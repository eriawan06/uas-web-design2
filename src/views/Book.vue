<template>
    <div>
        <v-container>
            <v-subheader class="title">{{book.title}}</v-subheader>
            <v-img v-if="book.cover" :src="getImage(book.cover)" height="auto"></v-img>
            <v-subheader>Information</v-subheader>
            <table class="v-data-table my-v-data-table">
                <tbody>
                    <!-- <tr>
                        <th class="text-xs-left">Author</th><td>{{book.author}}</td>
                    </tr>
                    <tr>
                        <th class="text-xs-left">Publisher</th><td>{{book.publisher}}</td>
                    </tr> -->
                    <tr>
                        <th class="text-xs-left text-left">Price</th><td class="text-right" v-if="book.price">Rp. {{
                            book.price.toLocaleString('id-ID')}}</td>
                    </tr>
                    <tr>
                        <th class="text-xs-left text-left">Weight</th><td class="text-right">{{book.weight}} Kg</td>
                    </tr>
                    <tr>
                        <th class="text-xs-left text-left">Stock</th><td class="text-right">{{book.stock}}</td>
                    </tr>
                    <tr>
                        <th class="text-xs-left text-left">Categories</th>
                        <td class="text-right">
                            <template v-for="category in book.categories" v-key="category.id">
                                {{category.name}}
                            </template>
                        </td>
                    </tr>
                </tbody>
            </table>
            <v-subheader>Description</v-subheader>
            <p class="body-2 text-justify">{{book.description}}</p>
            <div style="position:relative">
                <v-btn block color="success" @click="buy" :disabled="book.stock==0">
                    <v-icon>save_alt</v-icon> &nbsp;BUY
                </v-btn>
            </div>
        </v-container>
    </div>
</template>

<style>
.my-v-data-table {
    width: 100%;
}
</style>

<script>
import {mapActions} from 'vuex'
export default {
    data() {
        return {
            book: {},
        }
    },
    methods: {
        ...mapActions({
            addCart: 'cart/add',
            setAlert: 'alert/set',
        }),
        buy() {
            this.addCart(this.book)
            this.setAlert({
                status: true,
                text: 'Added to cart',
                type: 'success', 
            })
        },
    },
    created() {
        let slug = this.$route.params.slug
        this.axios.get('/books/slug/'+slug)
        .then((response) => {
            let book = response.data.data
            this.book = book
        }).catch((err) => {
            let responses = err.response
            console.log(responses)
        });
    },
}
</script>