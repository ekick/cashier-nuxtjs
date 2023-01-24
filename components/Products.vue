<template>
    <section id="product">
        <v-row align="center">
            <v-col>
                <v-autocomplete
                    label="Products"
                    placeholder="Start typing to search"
                    :search-input.sync="search"
                    :loading="isLoading"
                    :items="itemsSearch"
                    item-text="title"
                    item-value="id"
                    v-model="selectedSearch"
                    return-object
                    hide-no-data
                >
                </v-autocomplete>
            </v-col>
            <v-col >
                <v-menu>
                    <template v-slot:activator="{on: category}">
                        <v-btn
                            v-on="category"
                            color="primary"
                            >Category
                        </v-btn>
                    </template>
                    <v-list>
                        <v-list-item-group v-model="categoryId">
                            <v-list-item v-for="(category, index) in categories"
                                :key="index"
                                :value="category.id"
                                :disabled="category.id == categoryId">
                                <v-list-item-title>
                                    {{ category.title}}
                                </v-list-item-title>
                            </v-list-item>
                        </v-list-item-group>
                    </v-list>
                </v-menu>
            </v-col>
        </v-row>

        <v-row>
            <v-col v-for="(product, index) in filteredProducts" 
            cols="3"
            :key="index">
                <v-card :title="product.title" :ripple="true">
                    <v-card-actions>
                        <v-img :src="require(`@/assets/images/products/${product.thumbnail}`)"></v-img>
                    </v-card-actions>
                    <v-card-text align="center" class="product-title">
                        {{ product.title }}
                    </v-card-text>
                </v-card>
            </v-col>
        </v-row>
    </section>
</template>

<script>
// import {defineComponent} from '@vue/composition-api'
export default ({
    data() {
        return {
            categoryId: false,
            categories: [
                {id: false, title: 'Semua'},
                {id: 1, title: 'Makanan & Minuman'},
                {id: 2, title: 'Electronic'},
                {id: 3, title: 'Fashion'},
                {id: 4, title: 'Tools'},
            ],
            products: [
                { id: 1, title: 'Kacang Almond', thumbnail: 'almonds-g972a8d1c3_1920.jpg', price: 75000, categoryId: 1 },
                { id: 2, title: 'Smartphone Apel', thumbnail: 'apple-ga0d5ee5f5_1920.jpg', price: 43000, categoryId: 2 },
                { id: 3, title: 'Arduino | Raspberry board', thumbnail: 'arduino-gef5775dc6_1920.jpg', price: 5400000, categoryId: 2 },
                { id: 4, title: 'Minyak Mandi', thumbnail: 'bath-oil-gabb3b5c42_1280.jpg', price: 100000, categoryId: 1 },
                { id: 5, title: 'Jas dan Kemeja', thumbnail: 'black-and-white-g34733cb9e_1920.jpg', price: 350000, categoryId: 3 },
                { id: 6, title: 'Lenovo Thinkplus', thumbnail: 'c-d-x-PDX_a_82obo-unsplash.jpg', price: 2500000, categoryId: 2 },
                { id: 7, title: 'Lampu Fhilips', thumbnail: 'energy-saving-gdc7bacda8_1920.jpg', price: 170000, categoryId: 2 },
                { id: 8, title: 'Blouse Tosca', thumbnail: 'fashion-gcbf787177_1920.jpg', price: 500000, categoryId: 3 },
                { id: 9, title: 'Aneka Bumbu', thumbnail: 'food-g257dee53e_1920.jpg', price: 250000, categoryId: 1 },
                { id: 10, title: 'Headphone Estetik', thumbnail: 'headphones-g31ecdf442_1920.jpg', price: 6500000, categoryId: 2 },
                { id: 11, title: 'Ice Cream Pegasus', thumbnail: 'ice-cream-g2dfd5e7a8_1920.jpg', price: 3000000, categoryId: 1 },
                { id: 12, title: 'Kamera dengan custom lensa', thumbnail: 'lens-gca6ef8c7b_1920.jpg', price: 4100000, categoryId: 2 },
                { id: 13, title: 'Access point lynk sys', thumbnail: 'linksys-gf1713df70_1920.jpg', price: 2100000, categoryId: 2 },
                { id: 14, title: 'Macbook Air Pro', thumbnail: 'macbook-g32b8f1c6c_1920.jpg', price: 4100000, categoryId: 2 },
                { id: 15, title: 'Lipstick Rose', thumbnail: 'makeup-g984a2f330_1920.jpg', price: 2300000, categoryId: 3 },
                { id: 16, title: 'Pepsi Legend', thumbnail: 'pepsi-gfffb20bfd_1920.jpg', price: 4100000, categoryId: 1 },
                { id: 17, title: 'Stick PS5', thumbnail: 'playstation-gdc572c740_1920.jpg', price: 2100000, categoryId: 2 },
                { id: 18, title: 'Anggur Merah', thumbnail: 'red-wine-g01dfa492b_1920.jpg', price: 3000000, categoryId: 1 },
                { id: 19, title: 'Sepatu Hijau', thumbnail: 'running-shoes-gec6fce8f1_1920.jpg', price: 4100000, categoryId: 3 },
                { id: 20, title: 'Toolbox Screwdriver', thumbnail: 'toolbox-ga9dd00cc3_1920.jpg', price: 2300000, categoryId: 4 },
            ],
            search: null,
            isLoading: false,
            itemsSearch: [],
            selectedSearch: null,
        }
    },
    methods: {
        resetSearchCategory(){
            this.categoryId = false
        }
    },
    computed: {
        filteredProducts() {
            if (this.categoryId) {
                return this.products.filter(s => s.categoryId == this.categoryId)
            } else if (this.selectedSearch) {
                return this.products.filter(s => s.title == this.selectedSearch.title)
            }
            return this.products
        }
    },
    watch: {
        search(val) {
            // console.log(val)
            this.isLoading = true
            setTimeout(() => {
                this.itemsSearch = this.products.filter(e => {
                    this.isLoading = false
                    this.resetSearchCategory()
                    console.log(e.title)
                    return e.title
                })
            }, 500)
        }
    }
})
</script>

<style scoped>
    .product-title{
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    .img-product {
        /* max-width: 50vw; */
    }
    .search-form {
        width: 100%;
    }
</style>