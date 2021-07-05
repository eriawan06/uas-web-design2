<template>
  <div>
    <v-container grid-list-md>
      <v-subheader>
        All Books
      </v-subheader>
      <v-layout row wrap>
        <v-flex v-for="(book, index) in books" xs6 :key="index">
          <v-card :to="'/book/' + book.slug">
            <v-img :src="getImage('/books/' + book.cover)" height="150px">
              <v-container fill-height fluid pa-2>
                <v-layout fill-height>
                  <v-flex xs12 align-end flexbox>
                    <span
                      class="title white--text text-block"
                      v-text="book.title"
                    ></span>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-img>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn icon>
                <v-icon>favorite</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon>bookmark</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon>share</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
    <template>
      <div class="text-xs-center">
        <v-pagination v-model="page" @input="go" :length="lengthPage" :total-visible="4"></v-pagination>
      </div>
    </template>
  </div>
</template>

<style scoped>
  .text-block {
    position: absolute;
    bottom: 5px;
    left: 5px;
    background-color: black;
    padding-left: 5px;
    padding-right: 5px;
    opacity: 0.7;
    width: 100%;
  }
</style>

<script>
export default {
    data() {
        return {
            books: [],
            page: 0,
            lengthPage: 0,
        }
    },
    methods: {
        go() {
            let url = '/books'
            if(this.page > 0) url = url+'?page='+this.page

            this.axios.get(url)
            .then((res) => {
                let resData = res.data
                let books = resData.data
                this.lengthPage = resData.meta.last_page
                this.books = books
            }).catch((err) => {
                console.log(err.response)
            })
        },
    },
    created() {
        this.go()
    }
}
</script>