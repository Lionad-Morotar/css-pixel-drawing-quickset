<template>
<div class="pixel-generator-page">

    <!-- header -->
    <section class="header">
        <h3 class="title">CSS Monster Generator</h3>
        <h4 class="author">Lionad</h4>
    </section>

    <!-- content -->
    <main class="page-content">
        <section class="block block-pad">
            <template v-for="(r, ri) in config.pad.row">
                <div class="line-block" :key="'row-'+ri">
                    <template v-for="(l, li) in config.pad.line" >
                        <div class="pixel-block" 
                            :key="''+ri+'-'+li"
                            :style="[
                                {
                                    backgroundColor: calcBGColor(ri, li)
                                },
                                styles.pixelStyle
                            ]"
                            @click="togglePixel(ri, li)">
                        </div>
                    </template>
                </div>
            </template>
        </section>
        <section class="block block-res">
            <div id="monster" 
                :style="[
                    styles.pixelStyle,
                    styles.draw
                ]">
            </div>
        </section>
    </main>

    <!-- tool-container -->
    <section class="tool-con">
        <div class="tool-con-header">
            <h4 class="tool-con-title">TOOL PANEL</h4>
        </div>
    </section>

</div>
</template>

<script>

// NPM Package
// Project Package
// Project Page Components
// Project Components
// Project Config

export default {
    name: 'pixel-generator-page',
    components: {

    },

    data () {
        return {

            config: {

                pad: {
                    line: 0,
                    row: 0,
                    res: [],
                },

                res: {
                    width: '5em',
                    height: '5em',
                    boxShadow: '',
                },

                tool: {
                    
                    pointer: '#146cfe',
                    defaultPadColor: '#ccc',
                    defaultPointerColor: '#878787',
                    color: ['#146cfe'],
                    meter: 'em',
                    size: '3'
                    
                }

            },

            // DOM Style
            styles: {

                pixelStyle: {
                    width: '3em',
                    height: '3em'
                },

                draw: {
                    marginRight: '12em',
                    marginBottom: '12em',
                    boxShadow: '',
                    backgroundColor: ''
                },

            },

        }
    },

    computed: {

    },
    watch: {

        // 'config.pad': {
        //     handler: function (nv, ov) {
        //         if (
        //             nv.line != ov.line || 
        //             nv.row != ov.row )
        //         {
        //             console.log('adsf')
        //             this.initPadRes()
        //         }
        //     },
        //     deep: true
        // },

    },

    mounted () {

        this.config.pad.line = 5
        this.config.pad.row = 5
        this.initPadRes()

    },
    beforeDestroy () {

    },

    methods: {

        initPadRes () {
            let res = []

            Array.apply(null, { length: this.config.pad.row }).map(r => {
                let t = []
                Array.apply(null, { length: this.config.pad.line }).map(l => {
                    t.push({
                        bgColor: this.config.tool.defaultPadColor,
                    })
                    console.log('r, l :', r, l)
                })
                res.push(t)
            })

            this.config.pad.res = res
            console.log('config.pad.res :', this.config.pad.res)
        },

        togglePixel (r, l) {

            let handle = this.config.pad.res && this.config.pad.res[r] && this.config.pad.res[r][l]
            if (!handle) { return }

            let data = handle.bgColor

            if (r == 0 && l == 0) {
                this.styles.draw.backgroundColor =  data === this.config.tool.pointer ? this.config.tool.defaultPointerColor : this.config.tool.pointer
            }
            
            this.config.pad.res[r][l].bgColor = data === this.config.tool.pointer ? this.config.tool.defaultPadColor : this.config.tool.pointer

            this.draw()
        },
        draw () {
            let res = []

            this.config.pad.res.forEach((row, ri) => {
                row.forEach((item, li) => {
                    res.push(`
                        ${li*this.config.tool.size + this.config.tool.meter} 
                        ${ri*this.config.tool.size + this.config.tool.meter} 
                        ${item.bgColor === this.config.tool.defaultPadColor ? this.config.tool.defaultPointerColor : item.bgColor}
                    `)
                })
            })

            this.styles.draw.boxShadow = res.join(',\n')
        },

        calcBGColor (r, l) {
            return this.config.pad.res && 
                this.config.pad.res[r] && 
                this.config.pad.res[r][l] && 
                    this.config.pad.res[r][l].bgColor
        },

    },
}
</script>

<style lang="less" scoped>

.pixel-generator-page {
    display: block;
    background: #333;
    background: radial-gradient(#333, #000);
    width: 100%;
    height: 100%;
    overflow: hidden;
    font-size: 18px;
}

.header {
    padding: 10vh 0;
    width: 100%;
    text-align: center;
    color: #ddd;
    font-size: 3em;

    .title,
    .author {
        display: inline-block;
    }
    .author {
        margin-left: 4vw;
        font-size: 1vh;

        &::before {
            content: 'by ';
        }
    }
}

.page-content {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    margin: auto;
    width: 70vw;
    height: auto;

    .block {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-wrap: wrap;
        padding: .6em;
        width: 25vw;
        height: 25vw;
        border-radius: 3px;
        background-color: #888;

        &.block-res {
            // display: inline-block;
        }
    }

    .line-block {
        display: flex;
        justify-content: space-evenly;
        width: 100%;
    }
    .pixel-block {
        // width: 1em;
        // height: 1em;
        border-radius: 2px;
        background-color: #ccc;
        opacity: 1;
        transition: .3s;
        cursor: pointer;

            &:hover {
                opacity: .99;
                transition: .3s;
            }
    }
}

.tool-con {
    position: fixed;
    left: 0;
    bottom: 0;
    padding: 1em;
    width: 100vw;
    height: 20vh;
    background: #333;
    transform: translateY(17vh);
    transition: .35s ease-out;
    cursor: pointer;

    &:hover {
        transform: translateY(0);
    }

    .tool-con-header {
        width: 100%;

        .tool-con-title {
            font-size: 1.4em;
            color: #ccc;
        }
    }
}

</style>