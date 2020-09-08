<template>
    <div class="hello">
        <canvas id="snowflake" :width="width" :height="height"></canvas>
    </div>
</template>

<script>
import imgUrl from '../assets/timg.jpg'
export default {
    name: 'HelloWorld',
    props: {
        msg: String,
    },
    data() {
        return {
            snow: 500,
            arr: [],
            snowCtx: null,
            img: null,
            width: null,
            height: null
        }
    },
    created() {},
    mounted() {
        this.width =  window.innerWidth
        this.height = window.innerHeight
        let snow = document.querySelector('#snowflake')
        
        this.snowCtx = snow.getContext('2d')
        
       
        this.img = new Image()
        this.img.src = imgUrl
        for (var i = 0; i < this.snow; i++) {
            var obj = {
                x: Math.random() * this.width,
                y: Math.random() * this.height,
                r: Math.random() * 3 + 1,
            }
            this.arr.push(obj)
        }
        window.requestAnimationFrame(this.DrawSnow)
    },
    methods: {
        snowFall() {
            for (var i = 0; i < this.snow; i++) {
                var p = this.arr[i]
                p.y += Math.random() + 1
                if (p.y > this.height) {
                    p.y = 0
                }
                p.x += Math.random() + 1
                if (p.x > this.width) {
                    p.x = 0
                }
            }
        },
        DrawSnow() {
            this.snowCtx.clearRect(0, 0, this.width, this.height)
            this.snowCtx.drawImage(this.img, 0, 0, this.width, this.height)
            // snowCtx.fillStyle = "white"
            this.snowCtx.fillStyle = 'rgba(255,255,255, .77)'
            this.snowCtx.beginPath()
            for (let i = 0; i < this.snow; i++) {
                let p = this.arr[i]
                this.snowCtx.moveTo(p.x, p.y)
                this.snowCtx.arc(p.x, p.y, p.r, 0, 2 * Math.PI, false)
            }
            this.snowCtx.fill()
            this.snowFall()
            this.snowCtx.closePath()
            window.requestAnimationFrame(this.DrawSnow)
        },
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#snowflake {
  position:absolute;
  z-index:-2;
  display: block;
}
</style>
