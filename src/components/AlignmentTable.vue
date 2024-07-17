<script setup>
defineProps({
    contractId: {
        type: String,
        required: true
    },
    borrowerName: {
        type: String,
        required: true
    },
    grossAmount: {
        type: String,
        required: true
    }
})
</script>
<template>
    <ui5-table-cell class="bodyCell">
        <ui5-label>{{ contractId }}</ui5-label>
    </ui5-table-cell>
    <ui5-table-cell class="bodyCell">
        <ui5-label>{{ borrowerName }}</ui5-label>
    </ui5-table-cell>
    <ui5-table-cell class="bodyCell">
        <ui5-label>{{ grossAmount }}</ui5-label>
    </ui5-table-cell>
    <ui5-table-cell class="bodyCell">
        <ui5-label>
            <slot name="eligibilityValue"></slot>
            <slot name="alignmentValue"></slot>
        </ui5-label>
    </ui5-table-cell>
    <ui5-table-cell class="bodyCell">
        <ui5-label class="ccm-label">
            <slot name="ccmAligned"></slot>
            <slot name="ccmEligable"></slot>
        </ui5-label>
    </ui5-table-cell>
    <ui5-table-cell class="bodyCell">
        <ui5-label>
            <slot name="ccaAligned"></slot>
            <slot name="ccaEligable"></slot>
        </ui5-label>
    </ui5-table-cell>
</template>