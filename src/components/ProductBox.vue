<template>
    <div class="column is-3" v-bind:key="product.id">
                <div style="height: 100%;" class="box">
                    <figure class="image mb-4">
                        <img v-bind:src="product.get_thumbnail" :alt="product.name">
                    </figure>
                    <div>
                        <h3 class="is-size-4">{{ product.name }}</h3>
                        <p class="is-size-6 has-text-gray">₹ {{ product.price }}</p>
                    </div>

                    <router-link v-bind:to="product.get_absolute_url" class="button is-dark mt-4">View Details</router-link>
                </div>
            </div>
</template>

<script>
export default {
    name: 'ProductBox',
    props: {
        product: Object
    }
}
</script>