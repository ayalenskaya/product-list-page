<template>
    <LoadingSpinner v-if="loading" />
    <div v-else class="product-list">
        <div class="product-list__header">
            <h1 class="product-list__header-title">Добавление товара</h1>
            <select v-if="products.length >= 2" id="sort-criteria" v-model="sortCriteria" @change="sortProducts">
                <option value="default">По умолчанию</option>
                <option value="name">По названию</option>
                <option value="priceAsc">По возрастанию цены</option>
                <option value="priceDesc">По убыванию цены</option>
            </select>
        </div>
        <div class="product-list__content">
            <ProductForm :add-product="addProduct" />

            <div class="product-flex">
                <div v-if="products.length === 0" class="product-flex__empty">
                    Товаров нет.
                </div>
                <ProductItem v-else v-for="(product, index) in products" :key="index" :product="product" :index="index"
                    @remove-product="removeProduct" />
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';
import ProductForm from '@/components/ProductForm.vue';
import ProductItem from '@/components/ProductItem.vue';
import LoadingSpinner from '@/components/LoadingSpinner.vue';

export default {
    components: {
        ProductForm,
        ProductItem,
        LoadingSpinner
    },
    setup() {

        const products = ref(JSON.parse(localStorage.getItem('products')) || [
            { image: '/public/photo.png', name: 'Наименование товара', description: 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк', price: 10000 },
            { image: '/public/photo.png', name: 'Наименование товара', description: 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк', price: 10000 },
            { image: '/public/photo.png', name: 'Наименование товара', description: 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк', price: 10000 },
            { image: '/public/photo.png', name: 'Наименование товара', description: 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк', price: 10000 }
        ]);

        const loading = ref(true);


        setTimeout(() => {
            loading.value = false;
        }, 2000);

        const sortCriteria = ref('default');

        const sortProducts = () => {
            products.value.sort((a, b) => {
                switch (sortCriteria.value) {
                    case 'name':
                        return a.name.localeCompare(b.name);
                    case 'priceAsc':
                        return a.price - b.price;
                    case 'priceDesc':
                        return b.price - a.price;
                    default:
                        return 0;
                }
            });
        };

        const addProduct = (product) => {
            products.value.push(product);
            localStorage.setItem('products', JSON.stringify(products.value));
            sortProducts();
        };

        const removeProduct = (index) => {
            products.value.splice(index, 1);
            localStorage.setItem('products', JSON.stringify(products.value));
        };


        const activeInput = ref('');
        return {
            products,
            sortCriteria,
            sortProducts,
            addProduct,
            removeProduct,
            loading
        };
    }
};
</script>

<style lang="scss" scoped>
.product-list {
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    .product-list__header {
        display: flex;
        justify-content: space-between;
        align-items: center;

        select {
            border-radius: 4px;
            padding: 10px 16px;
            box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.1);
            background: #fffefb;
            font-weight: 400;
            font-size: 12px;
            color: #b4b4b4;
        }
    }

    .product-list__content {
        display: flex;

        .product-flex {
            display: flex;
            flex-wrap: wrap;
            justify-content: flex-start;
            gap: 16px;

            &__empty {
                text-align: center;
                padding: 30px;
                color: #888;
            }

        }
    }

    @media screen and (max-width: 768px) {

        .product-list__header {
            flex-direction: column;
            margin-bottom: 15px;
        }

        .product-list__content {
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }
    }
}
</style>