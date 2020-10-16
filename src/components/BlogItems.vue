<template>
    <div>
        <div class="cards_list" id="itemList" >
            <a href="#" class="cards_list_item" v-for="(blog,i) in itemsForList" :key="i">
                <div class="item_content">
                    <div class="item_content-image">
                        <img class="cards__item-logo" :src="blog.image_url" :alt="blog.company_name">
                    </div>                
                    <p class="cards__item-excerpt">{{blog.excerpt}}</p>
                    <a href="#" class="cards__item-link">
                        <span>Read more</span> 
                        <span class="read-more-arrow"><img :src="require(`@/assets/link_arrow.png`)" alt="" ></span>
                    </a>
                    <p class="cards__item-reading-time">
                        <span><img :src="require(`@/assets/clock.png`)" alt="" ></span>
                        <span>{{ calculateWordCount(blog.word_count) }} MIN READ</span>
                    </p>
                </div>                            
            </a>
        </div>

        <div class="pagination-container">
            <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="perPage"
            aria-controls="itemList"
            @click.native="$scrollToTop"
            ></b-pagination>
        </div>
    </div>
        
</template>

<script>
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

Vue.use(BootstrapVue)
Vue.use(IconsPlugin)

Vue.prototype.$scrollToTop = () => setTimeout(function(){window.scrollTo(0,0)},500);

export default {
    props: {
        allBlogs: Array,
    },
    data: function() {
        let data = {
            perPage: 6,
            currentPage: 1,
        };
        data = Object.assign({},data,this.allBlogs);
        return data;
    },
    computed: {
        rows() { // calculate number of rows
            return this.allBlogs.length
        },
        itemsForList() { // display X items per page
            return this.allBlogs.slice(
            (this.currentPage - 1) * this.perPage,
                this.currentPage * this.perPage,
            );
        }
    },
    methods: {
        calculateWordCount(val){ // calculate word count into X mins read time
            const WORDS_PER_MIN = 275;
            let timeToRead = Math.ceil(val/WORDS_PER_MIN);
            return timeToRead;
        }
    }
}
</script>

<style lang="scss">
.cards_list{
    display: flex;
    flex-wrap: wrap;
    .cards_list_item{
        background-color: #ffffff;
        border-radius: 5px;
        box-shadow: 0 0 10px 2px rgba(202, 202, 202, 0.3);
        transition: transform ease 0.3s, box-shadow ease 0.2s;   
        box-sizing: border-box;
        position: relative;
        &:after{
            content: '';
            display: block;
            padding-bottom: 100%;
        }
        &:hover{
            transform: translateY(-5px);
            box-shadow: 0px 0px 15px 2px rgba(176, 176, 176, 0.3);
            transition: transform ease 0.3s, box-shadow ease 0.2s;
            .item_content{
                .cards__item-link{
                    .read-more-arrow{
                        transform: translateX(6px);
                        transition: transform ease 0.2s;
                    }
                }
            }
        }    
        .item_content{
            display: flex;
            flex-direction: column;
            text-align: center;    
            box-sizing: border-box;  
            position: absolute;
            width: 100%;
            height: 100%;
            justify-content: space-between;       
            .item_content-image{
                height: 30%;
                margin-bottom: 15px;
            }
            .cards__item-excerpt{
                color: #5d7888;
                font-size: 16px;
                line-height: 22px;
                height: 40%;
                margin-top: 15px;
            }
            .cards__item-link{
                text-decoration: none;
                color: #1f77bc;                
                display: flex;
                justify-content: center;
                margin-bottom: 10px;
                span{
                    font-weight: 700;
                }
                .read-more-arrow{
                    transform: translateX(0);
                    transition: transform ease 0.2s;
                }
            }
            .cards__item-reading-time{
                color: #5d7888;
                letter-spacing: 0.833333px;
                display: flex;
                justify-content: center;
                margin-bottom: 0;
                img{
                    width: 14px;
                    height: 14px;
                    margin-right: 5px;
                }                
            } 
        }
    }
}
.pagination-container{
    display: flex;
    justify-content: center;
    .page-item.active .page-link{
        border-color: #259be4;
        background-color: #259be4;
    }
    .page-link{
        color: #259be4;
        &:hover{
            border: 1px solid #e2edfa;
            background-color: #f0f0f0;
        }
    }
}


@media screen and (min-width: 1280px){
    .cards_list{
        max-width: 1060px;
        margin: 10px auto 0 auto;
        .cards_list_item{
            width: calc(33% - 10px);
            margin-bottom: 30px;
            &:nth-child(3n-1){
                margin-left: 20px;
                margin-right: 20px;
            }
            .item_content{
                padding: 35px 45px;
                .cards__item-excerpt{
                    font-size: 16px;
                    line-height: 22px;
                }
                .cards__item-link{
                    font-size: 16px;
                    line-height: 16px;
                    img{
                        width: 21px;
                        height: 15px;
                        margin-left: 10px;
                    }
                }
                .cards__item-reading-time{
                    font-size: 10px;
                    line-height: 14px;
                    img{
                        width: 14px;
                        height: 14px;
                        margin-right: 5px;
                    }                
                }
                .item_content-image{
                    height: 30%;
                    width: inherit;
                    img{
                        height: 100%;
                    }
                }
            } 
        }
    }
    .pagination-container{        
        padding-bottom: 150px;
    }
}
@media screen and (min-width:768px) and (max-width: 1279px){
    .cards_list{       
        margin: 10px auto 0 auto;
        .cards_list_item{
            margin-bottom: 30px;
        }
    }
}
@media screen and (min-width:1024px) and (max-width: 1279px){
    .cards_list{
        width: 90vw;
        .cards_list_item{
            width: calc(32.95% - 10px);
            margin-bottom: 30px;
            &:nth-child(3n-1){
                margin-left: 20px;
                margin-right: 20px;
            }
            .item_content{
                padding: 22px 30px;
                .cards__item-excerpt{
                    font-size: 14px;
                    line-height: 20px;
                }
                .cards__item-link{
                    font-size: 14px;
                    line-height: 14px;
                    img{
                        width: 19px;
                        height: 13px;
                        margin-left: 10px;
                        margin-top: 2px;
                    }
                }
                .cards__item-reading-time{
                    font-size: 10px;
                    line-height: 14px;
                    img{
                        width: 14px;
                        height: 14px;
                        margin-right: 5px;
                    }                
                }
                .item_content-image{
                    height: 30%;
                    width: inherit;
                    img{
                        height: 100%;
                    }
                }
            } 
        }
    }
    .pagination-container{        
        padding-bottom: 150px;
    }
}
@media screen and (min-width:768px) and (max-width: 1023px){
    .cards_list{
        width: 90vw;
        justify-content: space-between; 
        .cards_list_item{
            width: calc(50% - 10px);
            margin-bottom: 30px;       
            .item_content{
                padding: 24px 30px;
                .cards__item-excerpt{
                    font-size: 16px;
                    line-height: 22px;
                }
                .cards__item-link{
                    font-size: 16px;
                    line-height: 16px;
                    img{
                        width: 21px;
                        height: 15px;
                        margin-left: 10px;
                        margin-top: 2px;
                    }
                }
                .cards__item-reading-time{
                    font-size: 10px;
                    line-height: 14px;
                    img{
                        width: 14px;
                        height: 14px;
                        margin-right: 5px;
                    }                
                }
                .item_content-image{
                    height: 25%;
                    width: inherit;
                    img{
                        height: 100%;
                    }
                }
            } 
        }
    }
}

@media screen and (max-width:767px){
    .cards_list{
        width: 90vw;
        justify-content: initial;
        margin: 20px auto 0 auto;
        .cards_list_item{
            flex: 0 0 100%;             
            margin-bottom: 30px;
            box-sizing: border-box;
            position: relative;
            &:after{
                content: '';
                display: block;
                padding-bottom: 100%;
            }           
            .item_content{
                position: absolute;
                width: 100%;
                height: 100%;
                justify-content: space-between;
                padding: 24px 30px;
                .cards__item-excerpt{
                    font-size: 16px;
                    line-height: 22px;
                }
                .cards__item-link{
                    font-size: 16px;
                    line-height: 16px;
                    img{
                        width: 21px;
                        height: 15px;
                        margin-left: 10px;
                        margin-top: 2px;
                    }
                }
                .cards__item-reading-time{
                    font-size: 10px;
                    line-height: 14px;
                    img{
                        width: 14px;
                        height: 14px;
                        margin-right: 5px;
                    }                
                }
                .item_content-image{
                    height: 25%;
                    width: inherit;
                    img{
                        height: 100%;
                    }
                }
            } 
        }
    }
    .pagination-container{        
        padding-bottom: 50px;
    }
}

@import 'node_modules/bootstrap/scss/bootstrap';
@import 'node_modules/bootstrap-vue/src/index.scss';
</style>