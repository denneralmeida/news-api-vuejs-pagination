<template>
  <div id="app" class="container mx-auto">
    <radio-category :categories="this.categories" 
      v-on:change-category="onChangeCategory"></radio-category>
      <div class="max-w-lg w-full lg:flex mx-auto py-2">
        <ul class="flex list-reset border border-grey-light rounded w-auto font-sans">
            <li><button class="block hover:text-white hover:bg-blue text-blue border-r border-grey-light px-3 py-2 "
             v-bind:class="this.currentPage === 0? 'opacity-50 cursor-not-allowed': ''" @click="this.previous">Previous</button></li>
             <li v-for="n in this.pageSize" :key="n"><button class="block hover:text-white hover:bg-blue px-3 py-2" @click="updatePage(n-1)" v-bind:class="currentPage === n-1? 'bg-blue text-white' : 'text-blue'"
              >{{n}}</button></li>
            <li><button class="block hover:text-white hover:bg-blue text-blue px-3 py-2"
             v-bind:class="this.currentPage === this.pageSize-1? 'opacity-50 cursor-not-allowed': ''" @click="this.next">Next</button></li>
        </ul>
    </div>
    <news-card v-for="(article, i) in this.visibleArticles" :article="article" :key="i"/>    
    
  </div>
</template>

<script>
import "./assets/css/app.css";
// import HelloWorld from './components/HelloWorld.vue'
// import Test from './components/Test.vue'
import NewsCard from "./components/NewsCard.vue";
// import CategoriesSelect from "./components/CategoriesSelect.vue";
import RadioCategory from "./components/RadioCategory.vue";

export default {
  name: "app",
  components: {
    "news-card": NewsCard,
    "radio-category": RadioCategory,
  },
  data() {
    return {
      articles: [],  
      visibleArticles: [],    
      category: 'science',
      key: 'HSHZrgisW4IelAHy3bnToEDtFKIOGG3y',
      categories: [
        'sports','books','world','technology','science','food','travel'
      ],
      total_results: 0,
      pageSize: 0,
      currentPage: 0
    };
  },
  created() {
    this.populateArticles();
	},
	methods: {
    onChangeCategory(category){      
      this.category = category;
      this.articles = [];
      this.populateArticles();
    },
    populateArticles() {      
      this.popNews().then(
        data => {
          this.reset();
          this.articles = data.results;
          this.total_results = data.num_results;
          
          this.pageSize = Math.round((this.total_results/5));
          this.updatePage(0);
          }
          
      );
    },
		async popNews() {
      const URL = `https://api.nytimes.com/svc/topstories/v2/${this.category}.json?api-key=${this.key}`;

      let response = await fetch(URL);
      let data = await response.json();
  
      return data;
    },
    next(){
      if(this.currentPage < this.pageSize-1){
        this.currentPage+=1;
        this.updatePage(this.currentPage);
      }
     },
    previous(){
      if(this.currentPage > 0){
        this.currentPage-=1;
        this.updatePage(this.currentPage);
      }
      
    },
    updatePage(num_page){
      this.currentPage = num_page;
      let index = num_page === 0 ? 0 : 1;
      this.visibleArticles = this.articles.slice((num_page * 5)+index, (num_page * 5) + 6);
    },
    reset(){
      this.total_results = 0;
      this.pageSize = 0;
      this.currentPage = 0;
    }
	}
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

button{
  cursor: pointer;
}

button.selected{
 outline: 3px dashed #f00;
}


</style>
