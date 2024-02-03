<template>
    <div class="product-flex__item">
        <div class="product-flex__item-image" :style="{ backgroundImage: getBackgroundImageStyle(product.image) }"></div>
        <h3 class="product-flex__item-name">{{ product.name }}</h3>
        <div class="product-flex__item-description">{{ product.description }}</div>
        <p class="product-flex__item-price">{{ formatPriceForDisplay(product.price) }} руб.</p>
        <img class="product-flex__item-delete" src="/delete.svg" alt="Delete Icon" @click="removeProduct(index)" />
    </div>
</template>
  
<script>

export default {
    props: {
        product: {
            type: Object,
            required: true
        },
        index: {
            type: Number,
            required: true
        }
    },
    emits: ['remove-product'],
    setup(props, { emit }) {
        const formatPriceForDisplay = (price) => {
            return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ');
        };

        const getBackgroundImageStyle = (imageUrl) => {
            return isValidImageUrl(imageUrl) ? `url(${imageUrl})` : 'url(/photo.png)';
        };

        const isValidImageUrl = (url) => {
            try {
                new URL(url);
                return true;
            } catch (_) {
                return false;
            }
        };

        const removeProduct = () => {
            emit('remove-product', props.index);
        };

        return {
            formatPriceForDisplay,
            getBackgroundImageStyle,
            removeProduct
        };
    }
};
</script>
  
<style lang="scss" scoped>
.product-flex__item {
    flex: 0 0 33.333%;
    max-width: 332px;
    height: 423px;
    display: flex;
    flex-direction: column;
    position: relative;
    background-color: #FFFEFB;
    border-radius: 4px;
    box-shadow: 0 6px 10px 0 rgba(0, 0, 0, 0.02),
        0 20px 30px 0 rgba(0, 0, 0, 0.04);
    text-align: start;

    &-image {
        border-radius: 4px;
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
        width: 332px;
        height: 200px;
        object-fit: cover;
    }

    &-name {
        font-size: 20px;
        font-weight: 600;
        line-height: normal;
        margin: 16px;
        color: #3f3f3f;
        word-wrap: break-word;
    }

    &-description {
        font-size: 16px;
        font-weight: 400;
        height: 80px;
        line-height: normal;
        margin: 0 16px 32px 16px;
        color: #3f3f3f;
        word-wrap: break-word;


    }

    &-price {
        font-size: 24px;
        font-weight: 600;
        line-height: normal;
        margin: 0 16px 24px 16px;
        color: #3f3f3f;
    }

    &-delete {
        position: absolute;
        top: -10px;
        right: -10px;
        cursor: pointer;
        z-index: 1;
        opacity: 0;
        transition: opacity 0.3s ease;
    }

    &:hover {
        .product-flex__item-delete {
            opacity: 1;
        }
    }
}

@media screen and (max-width: 768px) {
    .product-flex {
        flex-direction: column;
        justify-content: center;
    }
} 
</style>