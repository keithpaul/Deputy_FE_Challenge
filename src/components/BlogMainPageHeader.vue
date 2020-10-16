<template>
    <div class="cards__header">
        <h2>You're in good company</h2>
    </div>
</template>

<style lang="scss">
.cards__header{
    text-align: center;
    h2{
        color:#093049;
        font-weight: 700;
    }
}
@media screen and (min-width: 1280px){
    .cards__header{
        text-align: center;
        margin-top: 80px;
        margin-bottom: 34px;
        h2{
            font-size: 30px;
            line-height: 39px;
        }
    }
}
@media screen and (min-width:768px) and (max-width: 1279px){
    .cards__header{
        text-align: center;
        margin-top: 80px;
        margin-bottom: 34px;
        h2{
            font-size: 30px;
            line-height: 39px;
        }
    }
}
@media screen and (max-width:767px){
    .cards__header{
        text-align: center;
        margin-top: 50px;
        margin-bottom: 24px;
        h2{
            font-size: 28px;
            line-height: 36px;
        }
    }
}
</style>