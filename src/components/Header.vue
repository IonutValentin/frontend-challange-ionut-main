<script setup>
defineProps({
    totalExposure: {
        type: String,
        required: true
    },
    eligableAmount: {
        type: String,
        required: true
    },
    alignedAmount: {
        type: String,
        required: true
    }
})
</script>
<template>
    <ui5-title level="H3" wrapping-type="None" style="width: 15ch; margin: 0 0 10px 0">Header</ui5-title>
		<div class="alignment-overview-table">
			<div class="cell">
				<span class="title">Total Exposure </span>
				<span class="label">{{ totalExposure }}</span>
			</div>
			<div class="cell">
				<span class="title">Eligable Amount </span>
				<span class="label">{{ eligableAmount }}</span>
			</div>
			<div class="cell">
				<span class="title">Aligned Amount </span>
				<span class="label">{{ alignedAmount }}</span>
			</div>
		</div>
</template>