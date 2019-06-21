 <template>
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
 </template>