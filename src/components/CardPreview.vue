<script setup>

    import { ref } from 'vue';    
    import CardDetail from './CardDetail.vue';

    const props = defineProps(['card', 'namePrefix', 'onlyName']);

    const showDetail = ref({ on: false });

</script>

<template>

    <slot name="header"></slot>

    
    <div class="wrapper" @click="showDetail.on=true">
        <img v-if="!props.onlyName" :src="props.card.src" alt="" class="previewImg">
        <span class="cardName">{{ (props.namePrefix??'') + props.card.name }}</span>
    </div>

    <CardDetail :card="props.card" :show="showDetail"></CardDetail>

</template>

<style lang="less" scoped>

    .wrapper {
        display: flex;
        flex-direction: column;
        align-items: center;
        width: 100%;
        cursor: pointer;
        .previewImg {
            width: 100%;
            transition: .2s;
            &:hover {
                filter: drop-shadow(0 0 4px #009DBF);
            }
        }
        .cardName {
            display: inline-block;
            padding: 10px 0;
            text-align: center;
            transition: .2s;
            &:hover {
                color: #009DBF;
            }
        }
    }

</style>