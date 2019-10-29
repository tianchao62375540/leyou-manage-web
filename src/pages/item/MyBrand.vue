<template>
  <div>
    <v-layout class="px-5 pb-2" row wrap>
      <v-flex xs2>
        <v-btn color="info">新增品牌</v-btn>
      </v-flex>
      <v-spacer/>
      <v-flex xs4>
        <v-text-field hide-details id="testing" name="input-1" label="搜索" append-icon="search" v-model="key"/>
      </v-flex>
    </v-layout>
    <!-- :search="search" -->
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image" alt=""/></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="text-xs-center">
          <v-btn flat icon color="info">
            <v-icon>edit</v-icon>
          </v-btn>
          <v-btn flat icon color="error">
            <v-icon>delete</v-icon>
          </v-btn>
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  export default {
    name: "MyBrand",
    data() {
      return {
        headers: [
          {text: "品牌id", align: "center", sortable: true, value: "id"},
          {text: "品牌名称", align: "center",sortable: false, value: "name"},
          {text: "品牌LOGO", align: "center",sortable: false, value: "image"},
          {text: "品牌首字母", align: "center",sortable: true, value: "letter"},
          {text: "操作", align: "center",sortable: false},
          ],
        brands: [],
        pagination: {},
        totalBrands:15,
        loading: false,
        key:"",
      }
    },
    watch:{
      key(){
        this.pagination.page = 1;
        this.loadBrands();
      },
      pagination:{
        immediate:false,
        deep: true,
        handler(){
          console.log("===================================handler==============");
          this.loadBrands();
        }
      }
    },
    methods:{
      loadBrands(){
        this.loading=true;
        this.$http.get("/item/brand/page",{
          params:{
            page:this.pagination.page,
            rows:this.pagination.rowsPerPage,
            soryBy:this.pagination.sortBy,
            desc:this.pagination.descending,
            key:this.key
          }
        }).then(resp=>{
          console.log(resp);
          this.brands = resp.data.items;
          this.totalBrands = resp.data.total;
          this.loading=false;
        }).catch(error=> {
          this.brands=[];
          this.totalBrands=0;
          this.loading=false;
        });
      }
    }
  }
</script>

<style scoped>

</style>
