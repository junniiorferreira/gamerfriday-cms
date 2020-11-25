<template>
  <main class="aniversario-main-content">
    <img src="@/assets/images/banners/DESKTOP.jpg" class="top-banner" alt="Gamer Friday"/>
    <!-- Filters Bar -->
    <div class="page-left filters-menu__container" :class="{ open: isMenuOpen }">
        <div class="layer" @click="closeMenu"></div>
        <form class="filters-menu" :class="{ fixed: this.isMenuFixed }" id="filters-form">
            <div class="title">
                <h3>Filtros</h3>
            </div>
            <div class="search">
                <input
                    type="text"
                    name="product_name"
                    placeholder="Pesquisar"
                    v-model="filter.search"
                />
                <template v-if="filter">
                    <button type="button" @click="clearSearch">
                        <img v-if="!filter.search" class="icon search-icon" src="@/assets/images/icons/search.png" />
                        <img v-else class="icon clear-icon" src="@/assets/images/icons/close.png" />
                    </button>
                </template>
            </div>
            <div class="orderby">
                <select
                    name="orderby"
                    v-model="filter.orderby"
                >
                    <option value="" disabled selected hidden>Ordenar por</option>
                    <option value="name|ASC">Nome</option>
                    <option value="sale_initial_date|ASC">Ofertas mais próximas</option>
                    <option value="sale_initial_date|DSC">Ofertas mais distantes</option>
                    <option value="discount|ASC">Menor desconto</option>
                    <option value="discount|DSC">Maior desconto</option>
                    <option value="sale_price|ASC">Menor preço</option>
                    <option value="sale_price|DSC">Maior preço</option>
                </select>
            </div>
            <!-- <div class="filter-category">
                <h4>Categorias</h4>
                <template v-if="categories">
                    <label v-for="category in categories" :key="category.id">
                        <input
                            type="checkbox"
                            :value="category.slug"
                            v-model="filter.categories"
                        />
                        <div class="box"></div>
                        {{ category.name }}
                    </label>
                </template>
            </div> -->
            <div class="filter-category">
                <h4>Cadeiras</h4>
                <template v-if="chairs">
                    <label v-for="chair in chairs" :key="chair.id">
                        <input
                            type="checkbox"
                            :value="chair.slug"
                            v-model="filter.chairs"
                        />
                        <div class="box"></div>
                        {{ chair.name }}
                    </label>
                </template>
            </div>
            <div class="filter-category">
                <h4>Hardware</h4>
                <template v-if="hardwares">
                    <label v-for="hardware in hardwares" :key="hardware.id">
                        <input
                            type="checkbox"
                            :value="hardware.slug"
                            v-model="filter.hardwares"
                        />
                        <div class="box"></div>
                        {{ hardware.name }}
                    </label>
                </template>
            </div>               
            <div class="filter-category">
                <h4>Marcas</h4>
                <template v-if="brands">
                    <label v-for="brand in brands" :key="brand.id">
                        <input
                            type="checkbox"
                            :value="brand.slug"
                            v-model="filter.brands"
                        />
                        <div class="box"></div>
                        {{ brand.name }}
                    </label>
                </template>
            </div>
            <div class="filter-category faq">
                <p class="faq-title">Dúvidas?</p>
                <p class="faq-content">Acesse o FAQ especial para a Gamer Friday 2020 e tire suas dúvidas, <a href="#">clicando aqui</a>. </p>
            </div>
            <div class="buttons">
                <button type="button" class="apply-filters" @click="closeMenu">
                    Aplicar Filtros
                </button>
                <button type="button" class="clear-filters" @click="clearFilters">
                    Limpar Filtros
                </button>
            </div>
        </form>
    </div>
    <!-- End Filters Bar -->
    <!-- Page Content -->
    <div class="page-right content">
        <!-- Top Bar -->
        <div class="top-bar">
            <div class="left">
                <p class="title">
                    Oferta Gamer Friday
                </p>
                <p class="details">
                    Confira abaixo a listagem com todas as ofertas disponíveis.
                </p>
            </div>
            <div class="right">
                <div class="tooltip-button">
                    <img class="icon" src="@/assets/images/icons/warning.png" />
                    <div>
                        <span class="title mini">
                            Veja como funciona
                        </span>
                        <span class="details mini">
                            Oferta válida até o fim dos estoques
                        </span>
                    </div>
                </div>
                <div class="tooltip">
                    <h2>Como funciona?</h2>
                    <br/>
                    <p>
                        As ofertas de Gamer Friday da DT3 vão estar disponíveis para produtos selecionados até o fim dos estoques.
                        <br/>
                        É importante concluir o processo de pagamento da forma mais rápida possível, o produto só estará garantido após a confirmação dos dados de cobrança do cartão ou boleto.
                        <br/>
                        Produto no carrinho ou no checkout não garante reserva. Você estará concorrendo com outros clientes, então seja rápido ;D .
                    </p>
                </div>
            </div>
        </div>
        <!-- End Top Bar -->
        <!-- Filters Bar -->
        <div class="filters-bar" :class="{ fixed: isFiltersBarFixed }">
            <div class="alignment">
                <button @click="openMenu">
                    <img class="icon" src="@/assets/images/icons/filter.png">
                    Filtros
                </button>
            </div>
        </div>
        <!-- End Filters Bar -->
        <!-- Active Discounts -->
        <div class="active-discounts">
            <template v-if="hasActiveDiscounts">
                <product-list
                    :products="activeProducts"
                    @product-sale-finished="removeProduct($event, 'active')"    
                />
            </template>
            <template v-else>
                <div class="empty">
                    <h4>Nada por aqui</h4>
                    <mousedown />
                </div>
            </template>
        </div>
        <!-- End Active Discounts -->
        <!-- Middle Bar -->
        <!-- <div class="middle-bar">
            <div class="left">
                <p class="title">
                    Próximas ofertas
                </p>
                <p class="details">
                    Fique atento ao temporizador para aproveitar as melhores ofertas.
                </p>
            </div>
            <div class="right">
                <img class="icon" src="@/assets/images/icons/warning.png" />
                <div>
                    <p class="title">
                        Contagem regressiva
                    </p>
                    <p class="details">
                        A contagem regressiva de ofertas
                    </p>
                </div>
            </div>
        </div> -->
        <div class="faq-bar middle-bar">
            <div class="center">
                <p class="title">
                    Dúvidas? FAQ Gamer Friday
                </p>
                <p class="details">
                    Acesse o FAQ especial para a Gamer Friday 2020 e tire suas dúvidas, clicando aqui.
                </p>
            </div>
        </div>
        <div class="end-bar middle-bar">
            <div class="left">
                <p class="title">
                    Ofertas Finalizadas
                </p>
                <p class="details">
                    Fique atento às ofertas e finalize sua compra rapidamente. <br />
                    Produto no carrinho ou no checkout não garante a reserva do produto.
                </p>
            </div>
            <div class="right">
                <img class="icon" src="@/assets/images/icons/warning.png" />
                <div>
                    <p class="title">
                        Ofertas Finalizadas
                    </p>
                    <p class="details">
                        <a href="">Clique aqui e confira o FAQ para a Game Friday DT3 </a>
                    </p>
                </div>
            </div>
        </div>

        <!-- End Middle Bar -->
        <!-- Next Discounts -->
        <div class="next-discounts">
            <template v-if="willActiveDiscounts">
                <product-list 
                    :products="willActiveProducts"
                    @product-sale-finished="removeProduct($event, 'willActive')"
                    @product-sale-begin="makeProductActive($event)"
                />
            </template>
            <template v-else>
                <div class="empty">
                    <h4>Nada por aqui</h4>
                </div>
            </template>
        </div>
        <!-- End Next Discounts -->
    </div>
    <!-- End Page Content -->
  </main>
</template>

<script lang="ts">
  import { Component, Vue } from 'vue-property-decorator';
  import ProductList from './components/product-list.vue';
  import Mousedown from './components/mousedown.vue';

  @Component({
    components: {
      'product-list': ProductList,
      'mousedown':    Mousedown,
    },
  })
  export default class App extends Vue {
    public bannerHeight: number = 400;
    public topBarHeight: number = 105;
    public pageYOffset: number = 0;
    public categories: unknown = [];
    public brands: unknown = [];
    public chairs: unknown = [];
    public hardwares: unknown = [];

    public products = {
        active:     [],
        willActive: [],
    };

    public filter = {
        search: null,
        orderby: '',
        categories: [],
        brands: [],
        chairs: [],
        hardwares: [],
    };

    public isMenuOpen = false;


    public hasProducts(productList: any) {
        if ( !productList && !Array.isArray(productList)) return false;
        return productList.length;
    }

    public addProduct(product: any, type: 'active' | 'willActive'): void {
        if(!product || !type) return;
        this.products.active = [ ...this.products.active, product ] as never[];
    }

    public removeProduct(id: number, type: 'active' | 'willActive'): void {
        if(!id || !type) return;
        this.products[type] = this.products[type].filter((product: any) => product.id !== id);
    }

    public makeProductActive(id: number) {
        const product: any = this.products.willActive
            .filter((product: any) => product.id === id)
            .map((product: any) => ({ ...product, is_active: true  }))
            .pop();

        this.removeProduct(id, 'willActive');
        this.addProduct(product, 'active');
    }

    public objectToArray(object: any) {
        if (Array.isArray(object)) return object;
        if (typeof object !== 'object') return undefined;

        return Object.keys(object).map(function(key) {
            return object[key];
        });
    }

    public search(searchedTerm: any) {
        return function(product: any) {
            if (!searchedTerm) return true;
            return product.name.toLowerCase().includes(searchedTerm.toLowerCase());
        };
    }

    public clearSearch() {
        this.filter.search = null;
    }

    public transformBeforeSort(field: any) {
        const stringFields = ['name'];
        const dateFields   = ['sale_initial_date', 'sale_final_date'];

        function mapToLowerCase(product: any) {
            const newProduct = { ...product };
            if (!field) return newProduct;
            newProduct[field] = product[field].toLowerCase();
            return newProduct;
        }

        function mapToDate(product: any) {
            const newProduct = { ...product };
            if (!field) return newProduct;
            newProduct[field] = new Date(product[field]);
            return newProduct;
        }

        function mapToNumber(product: any) {
            const newProduct = { ...product };
            if (!field) return newProduct;
            newProduct[field] = parseFloat(product[field]);
            return newProduct;
        }

        return stringFields.includes(field)
            ? mapToLowerCase
            : dateFields.includes(field)
                ? mapToDate
                : mapToNumber;
    }

    public sortProducts(field: any, order: any) {
        return function(currentProduct: any, nextProduct: any) {
            if (!field || !order) return 0;

            if (order === 'ASC') {
                if (currentProduct[field] > nextProduct[field]) return 1;
                if (currentProduct[field] < nextProduct[field]) return -1;
                return 0;
            } else {
                if (currentProduct[field] < nextProduct[field]) return 1;
                if (currentProduct[field] > nextProduct[field]) return -1;
                return 0;
            }
        };
    }

    public transformAfterSort(originalProductList: any) {
        return function(product: any) {
            return originalProductList.filter((originalProduct: any) => originalProduct.id === product.id)[0];
        };
    }

    public filterProductsBy(field: string, selectedItems: any) {
        return (product: any) => {
            if (!selectedItems.length) return true;
            return selectedItems.includes(product[field].slug);
        };
    }

    public openMenu() {
        this.isMenuOpen = true;
        document.body.setAttribute('style', 'overflow: hidden');
    }

    public closeMenu() {
        this.isMenuOpen = false;
        document.body.setAttribute('style', 'overflow: auto');
    }

    public clearFilters() {
        this.filter = {
            search: null,
            orderby: '',
            categories: [],
            brands: [],
            chairs: [],
            hardwares: [],
        };
    }


    public trackPageYOffset() {
        this.pageYOffset = window.pageYOffset;

        window.addEventListener('scroll', () => {
            this.pageYOffset = window.pageYOffset;
        }, { passive: true, capture: true });
    }


    get isFiltersBarFixed() {
        return this.pageYOffset > (this.bannerHeight + this.topBarHeight);
    }

    get isMenuFixed() {
        return this.pageYOffset > this.bannerHeight
    }

    get activeProducts() {
        const searchedTerm       = this.filter.search;
        const selectedCategories = this.filter.categories;
        const selectedBrands     = this.filter.brands;
        const selectedChairs     = this.filter.chairs;
        const selectedHardwares  = this.filter.hardwares;
        const [field, order]     = this.filter.orderby
            ? this.filter.orderby.split('|')
            : ['name', 'ASC'];

        return this.products.active
            .filter(this.filterProductsBy('category', selectedCategories))
            .filter(this.filterProductsBy('brand', selectedBrands))
            .filter(this.filterProductsBy('chair', selectedChairs))
            .filter(this.filterProductsBy('hardware', selectedHardwares))                        
            .filter(this.search(searchedTerm))
            .map(this.transformBeforeSort(field))
            .sort(this.sortProducts(field, order))
            .map(this.transformAfterSort(this.products.active));
    }

    get willActiveProducts() {
        const searchedTerm       = this.filter.search;
        const selectedCategories = this.filter.categories;
        const selectedBrands     = this.filter.brands;
        const selectedChairs     = this.filter.chairs;
        const selectedHardwares  = this.filter.hardwares
        const [field, order]     = this.filter.orderby
            ? this.filter.orderby.split('|')
            : ['sale_initial_date', 'ASC'];

        return this.products.willActive
            .filter(this.filterProductsBy('category', selectedCategories))
            .filter(this.filterProductsBy('brand', selectedBrands))
            .filter(this.filterProductsBy('chair', selectedChairs))
            .filter(this.filterProductsBy('hardware', selectedHardwares))            
            .filter(this.search(searchedTerm))
            .map(this.transformBeforeSort(field))
            .sort(this.sortProducts(field, order))
            .map(this.transformAfterSort(this.products.willActive));
    }

    get hasActiveDiscounts() {
        return this.hasProducts(this.activeProducts);
    }

    get willActiveDiscounts() {
        return this.hasProducts(this.willActiveProducts);
    }

    mounted() {
        const products = require('./content/products.json').products;
        const categories = require('./content/categories.json').categories;
        const chairs = require('./content/chairs.json').chairs;
        const hardwares = require('./content/hardwares.json').hardwares;
        const brands = require('./content/brands.json').brands;
        
        const setIsActive = (product: any) => {
            const now = Math.trunc((new Date()).getTime() / 1000);
            const saleTime = Math.trunc(Date.parse(product.sale_initial_date) / 1000);
            const is_active = !(saleTime >= now);
            return { ...product, is_active };
        };

        const setId = (item: any, index: number) => ({ ...item, id: ++index });
        const getActiveDiscounts = (product: any) => product.is_active;
        const getWillActiveDiscounts = (product: any) => !product.is_active;

        this.products.active = products
            .map(setIsActive)
            .filter(getActiveDiscounts)
            .map(setId);

        this.products.willActive = products
            .map(setIsActive)
            .filter(getWillActiveDiscounts)
            .map(setId);

        this.categories = categories
            .map(setId);

        this.brands = brands
            .map(setId);

        this.chairs = chairs
            .map(setId);

        this.hardwares = hardwares
            .map(setId);    

        this.trackPageYOffset();
    }
}
</script>

<style>
</style>
