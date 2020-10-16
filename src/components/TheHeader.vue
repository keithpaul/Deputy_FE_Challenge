<template>
    <div class="header">
        <div class="header-inner">
            <div class="logo">
                <img :src="require(`@/assets/Deputy-White-HiRes.png`)" alt="Deputy Logo">
            </div>
        </div>        
    </div>
</template>

<style lang="scss">
.header{
    width: 100%;
    background-color: #093049;
    .logo{
        display: flex;
        align-items: center;
        height: 100%;
    }
}

@media screen and (min-width: 1280px){
    .header{
        width: 100%;
        height: 76px;
        .header-inner{
            width: 80%;
            max-width: 1060px;
            height: 100%;
            margin: 0 auto;
            .logo{        
                img{
                    max-height: 30px;
                    width: auto;
                }
            }
        }
    }
}

@media screen and (min-width:768px) and (max-width: 1279px){
    .header{
        width: 100%;
        height: 76px;
        .header-inner{
            width: 80%;
            height: 100%;
            margin: 0 auto;
            .logo{        
                img{
                    max-height: 30px;
                    width: auto;
                }
            }
        }
    }
}

@media screen and (max-width:767px){
    .header{
        width: 100%;
        height: 56px;
        .header-inner{
            width: 90%;
            height: 100%;
            margin: 0 auto;
            .logo{        
                img{
                    max-height: 25px;
                    width: auto;
                }
            }
        }
    }
}

</style>