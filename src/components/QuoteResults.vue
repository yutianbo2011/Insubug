<template>
  <div>
    <h2>Search Results</h2> 
    <div>
      <el-select v-model="sort" placeholder="Sort By" class="option-item">
        <el-option label="Sort by Price" value="price"></el-option>
        <el-option label="Sort by Name" value="name"></el-option>
      </el-select>
      <el-select v-model="bestSeller" placeholder="Best Seller" class="option-item">
        <el-option label="Yes" :value="true"></el-option>
        <el-option label="No" :value="false"></el-option>
      </el-select>
      <el-select v-model="type" placeholder="Type" class="option-item">
        <el-option label="Comprehensive" value="Comprehensive"></el-option>
        <el-option label="Fixed" value="Fixed"></el-option>
      </el-select>
      <el-select v-model="section" placeholder="Section" class="option-item">
        <el-option label="Travel Medical" value="Travel Medical"></el-option>
        <el-option label="International Travel Medical" value="International Travel Medical"></el-option>
        <el-option label="Student Medical" value="Student Medical"></el-option>
        <el-option label="J1 Medical" value="J1 Medical"></el-option>
      </el-select>
      <el-button @click="resetFilters">Reset Filters</el-button>
      <el-button @click="$emit('quote-again')">Quote Again</el-button>
    </div>
    <div>
      <el-card 
        v-for="item of this.displayQuotes"
        :key="item.id"
        class="quote-item">
        <h3>
          {{ item.name }}
        </h3>
        <span 
          v-if="item.bestSellers"
          class="best-seller">Best Seller</span>
        <el-row>
          <el-col :span="10">
            <span class="info-item"> ${{ item.price }} </span>
          </el-col>
          <el-col :span="14">
            <span class="info-item">{{ item.description }}</span>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="10">
            <span class="info-item"> {{ item.section }} </span>
          </el-col>
          <el-col :span="14">
            <span class="info-item"> {{ item.type }} </span>
          </el-col>
        </el-row>
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
 data() {
  return {
    sort: '',
    bestSeller: null,
    type: '',
    section: '',
    quotes : [
      {
        "id": 1,
        "price": 100,
        "name": "Atlas-america",
        "description": "Best comprehensive plan for visitors",
        "type": "Comprehensive",
        "section": "Travel Medical",
        "bestSellers": true
      },
      {
        "id": 2,
        "price": 150,
        "name": "Atlas-international",
        "description": "Best comprehensive plan for international travel",
        "type": "Comprehensive",
        "section": "International Travel Medical",
        "bestSellers": true
      },
      {
        "id": 3,
        "price": 75,
        "name": "IMG Patriot America",
        "description": "Comprehensive plan for visitors",
        "type": "Comprehensive",
        "section": "Travel Medical",
        "bestSellers": true
      },
      {
        "id": 4,
        "price": 250,
        "name": "IMG Patriot International",
        "description": "Comprehensive plan for international travel",
        "type": "Comprehensive",
        "section": "International Travel Medical",
        "bestSellers": false
      },
      {
        "id": 5,
        "price": 15,
        "name": "Visitor Care",
        "description": "Fixed plan for domestic travel",
        "type": "Fixed",
        "section": "Travel Medical",
        "bestSellers": true
      },
      {
      "id": 6,
      "price": 50,
      "name": "Visitor Secure",
      "description": "Fixed plan for domestic travel",
      "type": "Fixed",
      "section": "Travel Medical",
      "bestSellers": false
      },
      {
        "id": 7,
        "price": 87,
        "name": "Student travel",
        "description": "Best student travel plan",
        "type": "Fixed",
        "section": "Student Medical",
        "bestSellers": true
      },
      {
        "id": 8,
        "price": 69,
        "name": "Student comprehensive",
        "description": "Best student comprehensive plan",
        "type": "Comprehensive",
        "section": "Student Medical",
        "bestSellers": true
      },
      {
        "id": 9,
        "price": 154,
        "name": "J1 insurance",
        "description": "Best J1 comprehensive plan",
        "type": "Comprehensive",
        "section": "J1 Medical",
        "bestSellers": false
      },
      {
        "id": 10,
        "price": 200,
        "name": "J1 insurance comprehensive",
        "description": "J1 comprehensive plan",
        "type": "Comprehensive",
        "section": "J1 Medical",
        "bestSellers": true
      },
      {
        "id": 11,
        "price": 20,
        "name": "J1 insurance fixed",
        "description": "J1 fixed plan",
        "type": "Fixed",
        "section": "J1 Medical",
        "bestSellers": false
      },
      {
        "id": 12,
        "price": 200,
        "name": "Trawick Safe Travels",
        "description": "Top travel plan",
        "type": "Comprehensive",
        "section": "Travel Medical",
        "bestSellers": true
      },
      {
        "id": 13,
        "price": 250,
        "name": "Trawick Safe Travels Fixed",
        "description": "Top travel plan fixed",
        "type": "Fixed",
        "section": "Travel Medical",
        "bestSellers": true
      },
      {
        "id": 14,
        "price": 230,
        "name": "Trawick Safe Travels International",
        "description": "Top travel plan for international travel",
        "type": "Comprehensive",
        "section": "International Travel Medical",
        "bestSellers": true
      },
      {
        "id": 15,
        "price": 50,
        "name": "Insubuy Care plan",
        "description": "Top travel plan for insurance",
        "type": "Comprehensive",
        "section": "Travel Medical",
        "bestSellers": true
      }
    ]
  }
 },
  computed: {
    displayQuotes () {
      let res = [...this.quotes]
      if(this.bestSeller !== null){
        res = res.filter(item => item.bestSellers === this.bestSeller)
      }
      if(this.type){
        res = res.filter(item => item.type === this.type)
      }
      if(this.section){
        res = res.filter(item => item.section === this.section)
      }
      if(this.sort){
        let sortBy = this.sort
        res = res.sort((a, b)=>{
          return a[sortBy] > b[sortBy] ? 1 : -1
        })
      }
      return res
    }
  },
  methods:{
    resetFilters(){
      this.sort =  ''
      this.bestSeller = null
      this.type = ''
      this.section = ''
    }
  }
}
</script>

<style>
.quote-item{
  width: calc(50% - 30px);
    display: inline-block;
    height: 200px;
    position: relative;
    margin: 10px;
    float: left;
}
.best-seller{
  position: absolute;
    right: 30px;
    top: 30px;
    transform: rotate(20deg);
    opacity: 0.5;
    font-size: 14px;
    font-weight: 600;
}
.option-item{
  margin: 10px;
}
</style>