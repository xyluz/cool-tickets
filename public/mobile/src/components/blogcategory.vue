<template>
<div id="sidebar" class="xs-m4">
     <center><clip-loader v-if="loading" color="blue"></clip-loader></center>
   <div id="now-buzzing" class="xs-mb3">
      <br>
      <div id="now-buzzing" class="xs-mb3">
         <h3 class=" xs-text-2 xs-mb1">  <a href="#" target="_blank" class="button button--title xs-col-12 xs-text-left xs-mb1"> {{name}}</a></h3>
         <div id="vertical-feed" class="">
            <div class="vertical-specific-feed" id="mod-vertical-specific-feed-1">
               <div class="xs-text-left text-gray">
                  <ul class="list-unstyled" v-infinite-scroll="loadMore" infinite-scroll-disabled="busy" infinite-scroll-distance="10">
                     <li class="xs-col-12 xs-mb2"  v-for="(fre, index) in posts.fresh" v-if="index <=count">
                        <router-link v-bind:to="{ name: 'blogpost', params: { id: fre.id, title: respace(fre.title) }}"  class="bold text-gray">

                      <img class="buzz-image xs-block xs-mb05" :src="fre.image">
                      <P id="post-cat-m"> {{fre.category.name}} </P>
                      <h4 class="xs-text-4 lg-text-3">{{fre.title}}</h4>

                    </router-link>
                    <p id="share-m"> <span style="    font-size: 1.7em;
    font-weight: 900;
    vertical-align: sub;" class="ion-android-open"> </span> &nbsp {{shares('/blogpost/'+fre.id+'/'+respace(fre.title))}} <span style="font-weight: 100;">SHARES</span> <span style="font-weight: 100; opacity: .3;">/</span>  <span style="font-weight: 100;">{{timeago(fre.created_at)}}</span> </p>
                     </li>
                  </ul>
               </div>
            </div>
         </div>
         <br>
      </div>
   </div>
</div>
</template>
<script>
export default{
    props: ['id', 'name'],
    data:function(){
        return{

            posts: [],
            busy: false,
            count: 5,
            loading: true
        }
    },
    created: function(){

        this.cat_posts();
    },
    methods: {

      cat_posts: function(){
            axios.get("/blog_category/"+this.id)
            .then((response)=>{
            this.posts = response.data;
            this.loading = false;
            });
      },
      loadMore: function() {
      this.busy = true;
      this.count = this.count+10;
      this.busy = false;
    },
            respace: function(str){
              return str.replace(/ /g,"_");
            },
            shares: function(url){
                sharon.facebook.count(url=url,(err, count)=>{
                   return count;
                    //console.log('Whoa, we have ' + count + ' shares!');
                });

            },
            timeago: function (time){
                return moment(time).fromNow();
            }
    },
    computed: {
    },
    watch: {
        id: function(){
            this.cat_posts();
        }
    }
}
</script>
