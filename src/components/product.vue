<template>
    <a target="_blank" :href="product.link" class="product">
        <div class="image">
            <img :src="product.image_url" :alt="product.name">
        </div>
        <div class="name">
            <span class="type">{{ product.type }} </span> &nbsp;
            <span class="bold">{{ product.name }}</span>
        </div>
        <div class="price">
            <span class="old-value">De <span>R$ {{ product.regular_price | cash }}</span> por</span>
            <span class="new-value">R$ {{ product.sale_price | cash }}</span>
        </div>
        <span class="payment-type">
            <span>Valor à vista</span>
            <!-- <span>com cupom</span> -->
        </span>
        <div class="discount">
            <span class="label">
                Desconto
            </span>
            <span class="value">
                {{ product.discount }}%
            </span>
            <!-- <span class="info">
                com o cupom consumidordt3
            </span> -->
        </div>
        <span class="category">
            {{ product.category.name }}
        </span>
        <div class="alert green" v-if="product.is_active">
            <span class="content">
                <div class="block-extra">
                    <div class="title-content">Ver Mais</div>
                    <hr>
                    <div class="sub-content">Oferta válida até o fim dos estoques.</div>                  
                </div>                         
            </span>
        </div>
        <div class="alert black" v-else>
            <span class="content">
                <img class="icon" src="@/assets/images/icons/clock-red.png">
                <span class="time">
                    <countdown
                        :begin="product.sale_initial_date"
                        :end="product.sale_final_date"
                        @finished="$emit('finished', product.id)"
                        @begin="$emit('begin', product.id)"
                    />
                </span>
            </span>
        </div>
    </a>
</template>

<script lang="ts">
    import { Component, Prop, Vue } from 'vue-property-decorator';
    import Countdown from './countdown.vue'

    @Component({
        components: {
            'countdown': Countdown,
        },
        filters: {
            cash(value: number) {
                return value
                    .toFixed(2)
                    .replace('.', ',');
            }
        }
    })
    export default class Product extends Vue {
        @Prop()
        public product: any;
    }
</script>