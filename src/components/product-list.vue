<template>
    <div class="product-list">
        <product
            @finished="$emit('product-sale-finished', $event)"
            @begin="$emit('product-sale-begin', $event)"
            v-for="product in products"
            :product="product"
            :key="product.id"
        />
    </div>
</template>

<script lang="ts">
    import { Component, Prop, Vue } from 'vue-property-decorator';

    import Product from './product.vue';

    @Component({
        components: {
            'product': Product,
        }
    })
    export default class ProductList extends Vue {
        @Prop()
        public products: any;
    }
</script>