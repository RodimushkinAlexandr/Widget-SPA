<script setup lang="ts">
import type CategoryTypes from '@/types/Cost/Category';
import { computed } from 'vue';
import Icon from '../UI/icon.vue';
 
const props = defineProps<{
    category: CategoryTypes
    selected: string | null
}>()

const emit = defineEmits<{
    (e: "selected", value: string):void
}>()

const isSelected = computed(() =>  props.category.value == props.selected)

</script>

<template>
    <li @click="$emit('selected', selected)" :class="[{selected: isSelected}, category.color]" class="wallet__payment-item">
        <p>{{ category.value }}</p>
        <Icon :icon="category.icon" ></Icon>
    </li>
</template>

<style lang="scss" scoped>

.wallet {

    &__payment-item{
    
    margin: 0 auto;
    border-radius: 15px;
    overflow: hidden;
    width: 70px;
    height: 70px;
    cursor: pointer;
    box-shadow: 0px 5px 30px 2px rgba(0, 0, 0, 0.64); 
    transition: all .3s ease;
    display: flex;
    flex-direction: column-reverse;
    justify-content: center;
    text-align: center;
    background-color: none;
    box-shadow: 0px 5px 30px 2px rgba(0, 0, 0, 0.64);        
    transition: all ease .4s;

        &:hover {
            transform: scale(1.05);
        }

        input {
            text-align: inherit;
            cursor: pointer;
            font-size: 14px;
        }
        p {
            text-align: inherit;
            display: block;
        }
        &.selected {
            background-color: black;
        }
        &:hover{
            transform: scale(1.1);
            background-color: black;
        }
    }
}

@media (max-width: 480px) {
    .wallet {

    &__payment-wrapper_item{
            width: 60px;
            height: 60px;
        }
    }
}

</style>