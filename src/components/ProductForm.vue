<template>
    <div class="product-form">
        <form @submit.prevent="handleSubmit">
            <label for="product-name">Наименование товара<span class="required-dot"></span></label>
            <input id="product-name" v-model.trim="newProduct.name" type="text" placeholder="Введите наименование товара"
                required @input="validateField('name')" @focus="activeInput = 'name'" @blur="activeInput = ''"
                :class="{ 'input-error': errors.name && activeInput === 'name' }" maxlength="20">
            <div v-if="errors.name && activeInput === 'name'" class="error">{{ errors.name }}</div>

            <label for="product-description">Описание товара</label>
            <textarea id="product-description" v-model.trim="newProduct.description" placeholder="Введите описание товара"
                maxlength="100"></textarea>

            <label for="product-image">Ссылка на изображение товара<span class="required-dot"></span></label>
            <input id="product-image" v-model="newProduct.image" type="text" placeholder="Введите ссылку" required
                @input="validateField('image')" @focus="activeInput = 'image'" @blur="activeInput = ''"
                :class="{ 'input-error': errors.image && activeInput === 'image' }">
            <div v-if="errors.image && activeInput === 'image'" class="error">{{ errors.image }}</div>

            <label for="product-price">Цена товара<span class="required-dot"></span></label>
            <input id="product-price" v-model="newProduct.price" type="text" placeholder="Введите цену" required
                @input="(event) => { formatPrice(event); validateField('price'); }" @focus="activeInput = 'price'"
                @blur="activeInput = ''" :class="{ 'input-error': errors.price && activeInput === 'price' }" />
            <div v-if="errors.price && activeInput === 'price'" class="error">{{ errors.price }}</div>

            <button type="submit" @click="openAddProductModal" :disabled="isAddButtonDisabled">Добавить товар</button>
            <ProductModal :isVisible="isModalOpen" @close="closeModal">
                <h2>Товар добавлен!</h2>
            </ProductModal>
        </form>
    </div>
</template>
  
<script>
import { ref, computed } from 'vue';
import ProductModal from '@/components/ProductModal.vue';

export default {
    components: {
        ProductModal
    },
    props: {
        addProduct: {
            type: Function,
            required: true
        }
    },
    setup(props) {
        const newProduct = ref({
            image: '',
            name: '',
            description: '',
            price: null
        });

        const errors = ref({
            name: '',
            image: '',
            price: ''
        });

        const activeInput = ref('');

        const handleSubmit = () => {
            if (!isAddButtonDisabled.value) {
                props.addProduct({ ...newProduct.value });
                newProduct.value = {
                    image: '',
                    name: '',
                    description: '',
                    price: null
                };
            }
        };

        const isModalOpen = ref(false);

        const openAddProductModal = () => {
            isModalOpen.value = true;
        };

        const closeModal = () => {
            isModalOpen.value = false;
        };

        const validateField = (field) => {
            if (field === 'name') {
                errors.value.name = newProduct.value.name ? '' : 'Поле является обязательным';
            } else if (field === 'image') {
                errors.value.image = newProduct.value.image ? '' : 'Поле является обязательным';
            } else if (field === 'price') {
                errors.value.price = newProduct.value.price ? '' : 'Поле является обязательным';
            }
        };

        const formatPrice = (event) => {
            let value = event.target.value.replace(/\D/g, '');
            value = value.replace(/(\d)(?=(\d{3})+$)/g, '$1 ');
            newProduct.value.price = value ? parseFloat(value.replace(/\s/g, '')) : null;
            event.target.value = value;
            validateField('price');
        };
        const isAddButtonDisabled = computed(() => {
            return !newProduct.value.image || !newProduct.value.name || !newProduct.value.price;
        });

        return {
            newProduct,
            errors,
            activeInput,
            handleSubmit,
            validateField,
            formatPrice,
            isAddButtonDisabled,
            isModalOpen,
            openAddProductModal,
            closeModal
        };
    }
};
</script>
  
<style lang="scss" scoped>
.product-form {
    width: 332px;
    height: 440px;
    margin-right: 16px;
    padding: 24px;
    background-color: #FFFEFB;
    border-radius: 4px;
    box-shadow: 0px 6px 10px 0px rgba(0, 0, 0, 0.02), 0px 20px 30px 0px rgba(0, 0, 0, 0.04);

    form {
        display: flex;
        flex-direction: column;

        label {
            text-align: start;
            color: var(--Temp-Darks-Lesser, #49485E);
            font-size: 10px;
            margin-bottom: 4px;
            font-weight: 400;
            line-height: 12.57px;
            letter-spacing: -0.2px;
            position: relative;
        }

        .required-dot {
            position: absolute;
            transform: translateY(-50%);
            width: 4px;
            height: 4px;
            border-radius: 50%;
            background: #FF8484;
            flex-shrink: 0;
        }

        input,
        textarea {
            margin-bottom: 16px;
            border-radius: 4px;
            background: var(--darks-whites-white, #FFFEFB);
            box-shadow: 0px 2px 5px 0px rgba(0, 0, 0, 0.10);
            border: none;
            outline: none;
            width: 284px;
            padding: 10px 16px;
            flex-shrink: 0;
            color: var(--Greys-500, #B4B4B4);
            font-size: 12px;

            font-weight: 400;
            line-height: 15.08px;
        }


        input {
            height: 36px;

            &.input-error {
                border: 1px solid #FF8484;
                margin-bottom: 4px;
            }
        }


        textarea {
            height: 108px;
            resize: none;
        }
    }

    @media screen and (max-width: 768px) {
        .product-form {
            padding: 12px;
            margin-right: 0;

            form {

                input,
                textarea,
                button {
                    width: 100%;
                }
            }
        }
    }
}

.error {
    color: #FF8484;
    font-size: 8px;
    font-weight: 400;
    line-height: 10px;
    letter-spacing: -0.02em;
    text-align: left;
    margin-bottom: 2px;
}
</style>