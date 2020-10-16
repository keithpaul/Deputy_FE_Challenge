<template>
    <div>
        <div class="wrapper">
            <div class="my-container">
                <TheHeader />
                <div class="cards-container">
                    <BlogMainHeader />
                    <BlogFilters v-bind:allFilters="allFilters"
                                    @selectedFilters="filterBlogItems"/>
                    <BlogItems v-bind:allBlogs="filteredBlogs"/>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    components: { // code split
        BlogMainHeader: () => import(`@/components/BlogMainPageHeader.vue`),
        BlogFilters: () => import(`@/components/BlogFilters.vue`),
        BlogItems: () => import(`@/components/BlogItems.vue`),
        TheHeader: () => import(`@/components/TheHeader.vue`)
    },
    data: function(){
        return{
            blogs: [],
            filteredBlogs: [],
            allFilters: {},
        }
    },
    created(){
        fetch(`http://www.mocky.io/v2/5d1c07823400005200b5fae7`) // fetch data from API
            .then(response => {
                return response.json()
            })
            .then(data => {
                this.createBlogs(data) // send to createBlogs method for functionality
            })
    },
    methods: {
        createBlogs(blogs) { // create the blogs object and make a copy to be manipulated
            this.blogs = blogs;
            this.filteredBlogs = this.blogs;
            this.getUniqueValuesCount(this.blogs, ['industry', 'location', 'company_size','use_case'], ['Industries', 'Location', 'Company Size', 'Use case']);
        },
        getUniqueValuesCount(arr, keys, newKeys){ // Passed to Filters child - gets all unique values inside blogs object which key is passed as parameter (keys), then convert keys to 'newkeys' for rendering
            let result = {};
            for (const k of keys) result[k] = {items: {}};
            for (const item of arr) {                  
                for (const k of keys) {     
                    if(Array.isArray(item[k])){
                        for(let j = 0; j < item[k].length; j ++){
                            if (result[k]['items'][item[k][j]]) {                        
                                result[k]['items'][item[k][j]]++;
                            }
                            else {
                                result[k]['items'][item[k][j]] = 1;
                            }
                        }
                    }    
                    else{
                        if (result[k]['items'][item[k]]) {                        
                            result[k]['items'][item[k]]++;
                        }
                        else {
                            result[k]['items'][item[k]] = 1;
                        }
                    }       
                    
                }
            }

            // this is where we convert the keys into newkeys
            let newKeysResult = {};
            Object.keys(result).forEach( (key,i) => {            
                let newPair = { [newKeys[i]]: result[key] };
                newKeysResult = { ...newKeysResult, ...newPair }                
            });

            // here we add the isActive and count objects to be used in render in filter
            let objectKeys = Object.keys(newKeysResult)
            for (const key of objectKeys) {
                newKeysResult[key]['isActive'] = false;
                newKeysResult[key]['count'] = 0
            }
            return this.allFilters = newKeysResult;
        },
        filterBlogItems(params){  // filters the blog items with params passed from child component
            let currentBlogItems = this.blogs;
            let newFilteredBlogs = [];
            let paramsToCheck = [...params];
            if(!paramsToCheck.length){
                return this.filteredBlogs = this.blogs;
            }
            for(let i = 0; i < currentBlogItems.length; i++){                
                for (const value of Object.values(currentBlogItems[i])) {       
                    
                    if(Array.isArray(value)){
                        for(let j = 0; j < value.length; j++){
                            for(let k = 0; k < paramsToCheck.length; k++){
                                if(value[j] == paramsToCheck[k]) {                            
                                    newFilteredBlogs.push({...currentBlogItems[i]})
                                }
                            } 
                        }
                    }
                    for(let k = 0; k < paramsToCheck.length; k++){
                        if(value == paramsToCheck[k]) {                            
                            newFilteredBlogs.push({...currentBlogItems[i]})
                        }
                    }                    
                }
            }
            this.filteredBlogs = newFilteredBlogs;
        }
    }
}
</script>

<style lang="scss">
.wrapper{
    width: 100%;
    .my-container{
        width: 100%;
        margin: 0 auto;
        position: relative;
        &:before{
            content: '';
            position: fixed;
            width: 100%;
            background-color: #ffffff;
            top: 0;
            left: 0;
            z-index: -1;
        }
        &:after{
            content: '';
            position: fixed;
            width: 100%;
            height: 100%;
            background-color:#F3F5F6;
            top: 0;
            left: 0;
            z-index: -2;
        }
        .cards_container{
            width: 100%;
            margin: 0 auto;
        }
    }

}

@media screen and (min-width: 1280px){
    .wrapper{        
        .my-container{
            &:before{
                height: 50vh;
            }   
            .cards_container{
                max-width: 1200px;
                padding-top: 10%;
            }
        }
    }
}
@media screen and (min-width:1024px) and (max-width: 1279px){
    .wrapper{        
        .my-container{
            &:before{
                height: 40vh;
            }   
            .cards_container{
                padding-top: 10%;
            }
        }
    }
}
@media screen and (min-width:768px) and (max-width: 1023px){
    .wrapper{        
        .my-container{
            &:before{
                height: 50vh;
            }   
            .cards_container{
                padding-top: 10%;
            }
        }
    }
}
@media screen and (max-width: 767px){
    .wrapper{        
        .my-container{
            &:before{
                height: 70vh;
            }   
            .cards_container{
                padding-top: 10%;
            }
        }
    }
}
</style>