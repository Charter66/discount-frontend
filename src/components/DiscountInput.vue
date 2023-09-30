<template>
    <div class="input-container">
        <input v-model="discountCode" placeholder="Enter Discount Code">
        <button @click="applyDiscount">Apply Discount</button>
    </div>
</template>

<script>
export default {
    props: ['totalSum'],  // a. Receive the total sum as a prop

    data() {
        return {
            discountCode: '',
            discountPercentage: 0
        }
    },

    methods: {
        applyDiscount() {

            fetch('http://localhost/back-end-discount/applyDiscount.php', { 
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ code: this.discountCode }),
            })
            .then(response => {
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                return response.json();
            })
            .then(data => {
                if (data.status === 'success') {
                    this.discountPercentage = parseFloat(data.percentage);
                    let discountedSum = this.totalSum * (1 - this.discountPercentage / 100);
                    this.$emit('updateTotal', discountedSum); // b. Emit event to update total sum in Cart
                } else {
                    console.error(data.message);  // Handle invalid discount code scenario
                }
            });
        }
    }
}
</script>
