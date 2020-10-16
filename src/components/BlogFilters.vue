<template>
    <div class="cards__filters">
        <div class="filter">
            <p>FILTER BY</p>
        </div>
        <div class="filter" v-for="(filtersList,filterName) in allFilters" :key="filterName">
            <button @click="filtersList.isActive = !filtersList.isActive;closeOthers(filterName)" class="filter-button">
                <span>{{filterName}}</span><span><img :src="require(`@/assets/chevron-down.png`)" alt=""></span>
            </button>
            <ul class="filter-list" v-on-clickaway="() => removeActive(filterName)" :class="{active:filtersList.isActive}" :value="filterName">
                <li class="filter-options">
                    <span class="filters-selected-amount">{{ filtersList.count }} selected</span> 
                    <span class="filters-clear-button" @click="clearFilters(filtersList.items);filtersList.count = 0">Clear</span>
                </li>
                <li class="filter--item" v-for="(value,optionName) in filtersList.items" 
                    @click="addSelectedFilter(filtersList.items,optionName);checkThis($event, filterName)"  
                    :key="optionName" 
                    :value="optionName">
                    <span class="filter__checked"><img :src="require(`@/assets/check.png`)" alt=""></span>
                    {{optionName}} ({{value}})
                </li>
            </ul>
        </div>
    </div>
</template>


<script>
import { mixin as clickaway } from 'vue-clickaway2';

export default {
    mixins: [ clickaway ],
    props: {
        allFilters: Object,
    },
    data: function() {
        let data = {
            filtersSelected: []
        };
        data = Object.assign({},data,this.allFilters);
        return data;
    },
    methods: {
        addSelectedFilter(list, val){ // add selected filter to filtersSelected array
            let currentFilters = this.filtersSelected;
            if(currentFilters.includes(val)){
                currentFilters.splice(currentFilters.indexOf(val),1);
            }
            else{
                currentFilters.push(val);
            }
            this.filtersSelected = currentFilters;
            this.$emit("selectedFilters", this.filtersSelected); // send back up to parent method selectedFilters to re filter blogs
        },
        clearFilters(val){ // remove all filters from clicked dropdown from filterArray
            let currentFilters = this.filtersSelected;
            let filtersToClear = Object.keys(val);
            let filteredArr = currentFilters.filter((el) => {
                return filtersToClear.indexOf(el) === -1;
            })
            this.filtersSelected = filteredArr;
            this.clearAllSelected(filtersToClear);
            this.$emit("selectedFilters", this.filtersSelected); // send back up to parent method selectedFilters to re filter blogs
        },
        checkThis(e, filterName) { // add/remove selected class to filter element
            if(e.srcElement.classList.contains('selected')){
                this.allFilters[filterName].count -=1;
            }
            else{
                this.allFilters[filterName].count +=1;
            }
            e.srcElement.classList.toggle('selected');
        },
        clearAllSelected(val){ // clear all selected classes from dropdown list
            let allSelected = document.querySelectorAll('.selected');
            for(let i = 0; i < allSelected.length; i++){
                for(let j = 0; j < val.length; j++){
                    if(allSelected[i].getAttribute('value')==val[j]){
                        allSelected[i].classList.toggle('selected')
                    }
                }
            } 
        },
        removeActive(el){ // remove active class from current dropdown (hide it)  
            this.allFilters[el].isActive = false;
        },
        closeOthers(el){ // close all other dropdown currently open (hide them)
            let currentFilters = this.allFilters;
            Object.keys(currentFilters).map(x=>{ if(x != el){return currentFilters[x].isActive = false}});
            this.allFilters = currentFilters;
        }
    },
}
</script>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style lang="scss">

.cards__filters{    
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    .filter{
        position: relative;
        p{
            font-weight: lighter;
            color: #6e8593;
            text-transform: uppercase;
        }
        .filter-button{
            border: 1px solid #ccc;
            background-color: #ffffff;
            color: #6e8593;
            border-radius: 5px;
            text-align: left;          
            cursor: pointer;  
            span{
                &:nth-of-type(1){
                    float: left;
                }
                &:nth-of-type(2){
                    float: right;
                    img{
                        width: 10px;
                    }
                }
                display: flex;
                align-items: center;
                height: 100%;
            }
        }
        ul{
            position: absolute;
            z-index: 99;
            background-color: #ffffff;
            border-radius: 4px;
            border: 1px solid #CED6DB;
            padding: 0 20px 20px 20px;
            box-sizing: border-box;
            visibility: hidden;
            transition: visibility ease 0.2s;
            &.active{
                visibility: initial;
                transition: visibility ease 0.2s;
            }
            li{
                width: 100%;
                height: 44px;
                border-radius: 4px;
                color: #5D7888;
                background-color: #F3F5F6;
                font-size: 14px;
                margin: 2px auto;
                display: flex;
                align-items: center;
                position: relative;
                padding-left: 31px;
                box-sizing: border-box;    
                transition: background-color ease 0.2s, transform ease 0.2s; 
                transform: translateX(0);           
                &:hover{
                    cursor: pointer;
                }
                .filter__checked{
                    display: none;
                }
                &.filter--item{                    
                    &:hover{
                        color: #FFFFFF;
                        background-color: #093149;
                        transition: background-color ease 0.2s;
                    }
                    &.selected{
                        color: #FFFFFF;
                        background-color: #093149;
                        transform: translateX(5px);
                        transition: background-color ease 0.2s, transform ease 0.2s;
                        .filter__checked{
                            display: block;
                            position: absolute;
                            left: 5px;

                        }
                    }
                }
                
                &.filter-options{
                    justify-content: space-between;
                    padding: 0;
                    background-color: #ffffff;
                }
                .filters-selected-amount{
                    color: #5D7888;
                    font-size: 14px;
                }
                .filters-clear-button{
                    color: #259BE4;
                    text-decoration: underline;
                    font-weight: 700;
                }
            }
            
        }
    }
}

@media screen and (min-width: 1280px){
    .cards__filters{
        margin: 0 auto 31px auto;
        justify-content: center;
        max-width: 1060px;;
        width: 100%;
        .filter{      
            ul{                
                top: 52px;
                left: 0;
                width: 286px;
            }      
            &:nth-of-type(1){
                margin-right: 20px;
            }
            &:nth-of-type(n+2){
                flex: 0 0 200px;
                margin: 0 10px;
            }
            p{
                font-size: 16px;
            }
            .filter-button{
                font-size: 18px;
                height: 42px;
                width: 200px;
                padding: 0 15px;
            }
            
        }
    }
}
@media screen and (min-width:1024px) and (max-width: 1279px){
    .cards__filters{
        margin-bottom: 31px;
        justify-content: space-between;
        width: 90vw;
        margin: 0 auto;
        .filter{       
            &:not(:last-child){
                ul{
                    left: 0;
                }    
            }
            &:last-child{
                ul{
                    right: 0;
                } 
            }     
            ul{                
                top: 52px;
                width: 286px; 
            } 
            &:nth-of-type(1){
                flex: 0 0 15%;
                text-align: center; 
                margin-bottom: 20px;
            }
            &:nth-of-type(n+2){
                flex: 0 0 20%;
                margin-bottom: 30px;
            }
            p{
                font-size: 14px;
            }
            .filter-button{
                font-size: 16px;
                height: 42px;
                width: 100%;
                padding: 0 15px;
            }
            
        }
    }
}
@media screen and (min-width:768px) and (max-width: 1023px){
    .cards__filters{
        margin-bottom: 31px;
        justify-content: space-between;
        width: 90vw;
        margin: 0 auto;
        .filter{       
            p{
                margin-bottom: 5px;
            }
            &:not(:last-child){
                ul{
                    left: 0;
                }    
            }
            &:last-child{
                ul{
                    right: 0;
                } 
            }     
            ul{                
                top: 52px;
                width: 286px; 
            } 
            &:nth-of-type(1){
                flex: 0 0 100%;
                text-align: left; 
            }
            &:nth-of-type(n+2){
                flex: 0 0 22%;
                margin-bottom: 30px;
            }
            p{
                font-size: 14px;
            }
            .filter-button{
                font-size: 16px;
                height: 42px;
                width: 100%;
                padding: 0 15px;
            }
            
        }
    }
}
@media screen and (max-width: 767px){
    .cards__filters{
        margin-bottom: 31px;
        justify-content: space-between;
        width: 90vw;
        margin: 0 auto;
        .filter{   
            p{
                margin-bottom: 5px;
            }         
            ul{                
                top: 52px;
                width: 286px; 
                left: 0;
            } 
            &:nth-of-type(1){
                flex: 0 0 100%;
                text-align: left; 
            }
            &:nth-of-type(n+2){
                flex: 0 0 100%;
                margin-bottom: 5px;
            }
            p{
                font-size: 14px;
            }
            .filter-button{
                font-size: 16px;
                height: 42px;
                width: 100%;
                padding: 0 15px;
            }
            
        }
    }
}
</style>