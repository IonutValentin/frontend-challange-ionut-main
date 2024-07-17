<script setup>
import { reactive, onMounted, onBeforeMount, ref } from "vue";
import Header from "./components/Header.vue";
import AlignmentTable from "./components/AlignmentTable.vue";

import { setTheme } from "@ui5/webcomponents-base/dist/config/Theme";
import '@ui5/webcomponents-base/dist/features/F6Navigation';
import "@ui5/webcomponents/dist/Table.js";
import "@ui5/webcomponents/dist/TableRow.js";
import "@ui5/webcomponents/dist/TableCell.js";
import "@ui5/webcomponents/dist/TableHeaderRow.js";
import "@ui5/webcomponents/dist/TableHeaderCell.js";
import "@ui5/webcomponents/dist/Title.js";

const loan = reactive({ 
  totalExposure: "", 
  eligableAmount: "", 
  alignedAmount: "",
  productListWithAlignment: "",
});

onBeforeMount(() => {
  fetch("./taxonomy/loans/loan.json")
    .then((response) => response.json())
    .then((data) => {
      loan.totalExposure = data.metadata.grossCarryingAmountTotal.formatted;
      loan.eligableAmount = data.metadata.eligabilityTotal.formatted;
      loan.alignedAmount = data.metadata.alignmentTotal.formatted;
      loan.productListWithAlignment = data.productListWithAlignment;
    });
});

setTheme("sap_horizon");
</script>

<template>
  <div class="app">
		<Header :totalExposure="loan.totalExposure" 
            :alignedAmount="loan.alignedAmount"
            :eligableAmount="loan.eligableAmount"
    />


		<ui5-title level="H3" wrapping-type="None" style="width: 15ch; margin: 100px 0 10px 0">Loan Alignment</ui5-title>
		<ui5-table id="loan-alignment-table">
			<ui5-table-header-row slot="headerRow">
				<ui5-table-header-cell width="250px">Loan Id</ui5-table-header-cell>
				<ui5-table-header-cell width="250px">Partner Name</ui5-table-header-cell>
				<ui5-table-header-cell width="250px">Exposure</ui5-table-header-cell>
				<ui5-table-header-cell width="250px">Aligned & Eligable Total</ui5-table-header-cell>
				<ui5-table-header-cell width="250px">CCM Aligned & Eligable</ui5-table-header-cell>
				<ui5-table-header-cell width="250px">CCA Aligned & Eligable</ui5-table-header-cell>
			</ui5-table-header-row>
			<ui5-table-row class="bodyRow" v-for="(data,i) in loan.productListWithAlignment" :key="i">
				<AlignmentTable :contractId="data.product.contractId"
								:borrowerName="data.product.borrower.name"
								:grossAmount="data.product.grossCarryingAmount.formatted"
				>
					<template v-slot:eligibilityValue>
						{{ data.alignmentKPIList.reduce((acc, curr) => acc + curr.eligibilityValue.amount, 0) }} of
					</template>
					<template v-slot:alignmentValue>
						{{ data.alignmentKPIList.reduce((acc, curr) => acc + curr.alignmentValue.amount, 0) }} EUR
					</template>
				
					<template v-slot:ccmAligned>
						{{ data.alignmentKPIList.map( el => el.dataPoints.filter((item) => 
                        	item.environmentalObjective == "CCM")
                        	.reduce((acc, curr) => acc + curr.eligableValue.amount, 0))
                        	.reduce((acc, curr) => acc + curr) 
                    	}} of
					</template>
					<template v-slot:ccmEligable>
						{{ data.alignmentKPIList.map( el => el.dataPoints.filter((item) => 
                        	item.environmentalObjective == "CCM")
                        	.reduce((acc, curr) => acc + curr.alignedValue.amount, 0))
                        	.reduce((acc, curr) => acc + curr)
                    	}} EUR
					</template>
				
					<template v-slot:ccaAligned>
						{{ data.alignmentKPIList.map( el => el.dataPoints.filter((item) => 
                        	item.environmentalObjective == "CCA")
                        	.reduce((acc, curr) => acc + curr.eligableValue.amount, 0))
                        	.reduce((acc, curr) => acc + curr) 
                    	}} of
					</template>
					<template v-slot:ccaEligable>
						{{ data.alignmentKPIList.map( el => el.dataPoints.filter((item) => 
                        	item.environmentalObjective == "CCA")
                        	.reduce((acc, curr) => acc + curr.alignedValue.amount, 0))
                        	.reduce((acc, curr) => acc + curr)
                    	}} EUR
					</template>
				</AlignmentTable>
			</ui5-table-row>
		</ui5-table>
	</div>
</template>

<style>
html, body {
	padding: 0;
	margin: 0;
	height: 100%;
  background: #f5f6f7;
}

.app {
	height: 100%;
	background-color: var(--sapBackgroundColor);
}

.alignment-overview-table {
	background: white;
	display: flex; 
	flex-wrap: wrap; 
	justify-content: end; 
	width: 620px;
  font-size: 14px;
  padding: 0 10px;
	box-shadow: 0 1px 0 0 rgb(228, 228, 228);
}

.alignment-overview-table .cell {
	width: 300px;
	padding: 15px 0;
    box-shadow: 0 1px 0 0 rgb(228, 228, 228);
}

.alignment-overview-table .title {
	font-weight: 600;
	color:#1d2d3e;
}
</style>