<template>
    <div>
        <h2>Cart</h2>
        <ul>
            <li v-for="(item, index) in items" :key="index">
                {{ item.name }} - ${{ item.price }}
            </li>
        </ul>
        <strong>Total: ${{ discountedTotal !== null ? discountedTotal.toFixed(2) : total }}</strong>
                <DiscountInput :totalSum="total" @updateTotal="discountedTotal = $event" />

    </div>
</template>

<script>
import DiscountInput from './DiscountInput'
export default {
    data() {
    return {
        discountedTotal: null  // Initializes as null, so we know when it has been set
    }
},
    name: 'CartOne',
    props: {
    items: {
        type: Array,
        default: () => []
    }
},
computed: {
    total() {
        const sum = this.items.reduce((acc, product) => {
            // Convert the product.price to a floating point number before adding
            return acc + parseFloat(product.price);
        }, 0);
        return sum.toFixed(2);
    }
},

components: {
    DiscountInput
}

}
</script>

<style scoped>
/* Optional: Styles for the cart */
</style>
