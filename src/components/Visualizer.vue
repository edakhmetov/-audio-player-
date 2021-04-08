<template>
    <div class="visualizer">
        <canvas width="550" height="300" ref="canvas">
        </canvas>
    </div>
</template>

<script>
 export default {
    name: 'Visualizer',
    props: {
        audioAnalyser: {
            type: AnalyserNode,
            default: null
        }
    },
    data() {
        return {
            myCanvas: null,
            myCtx: null
        }
    },
    mounted: function() {
        this.myCanvas = this.$refs.canvas;
        this.myCtx = this.myCanvas.getContext('2d')
        this.mainLoop();
    },
    methods: {
        mainLoop () {
            // console.log('visuzlier');
            // const frqBits = this.audioAnalyser.frequencyBinCount
            // const fftSize = this.audioAnalyser.fftSize
            // const dataArray = new Uint8Array(frqBits)
            // var timeFrequencyData = new Uint8Array(fftSize)
            // const numBars = 64
            // const gapSize = 4
            // const step = Math.round(frqBits * 0.85 / (numBars)) // Songs do not contain the higher frequencies so cut them out
            // var xPos = 0
            // var barWidth = this.myCanvas.width / (numBars)
            // this.audioAnalyser.getByteFrequencyData(dataArray)
            // this.audioAnalyser.getByteTimeDomainData(timeFrequencyData)
            // this._fillCanvasBG()
            // dataArray.forEach((data, indexPos) => {
            //     if (indexPos % step) return
            //     const bits = Math.round(dataArray.slice(indexPos, indexPos + step)
            //     .reduce((v, t) => t + v, 0) / step)
            //     const barHeight = bits / 255 * this.myCanvas.height
            //     this._drawBars(xPos, barHeight, barWidth)
            //     xPos += barWidth + gapSize
            // })
            // requestAnimationFrame(this.mainLoop)
        },
        _fillCanvasBG: function () {
            // Resets the canvas to black
            const w = this.myCanvas.width
            const h = this.myCanvas.height
            this.myCtx.fillStyle = 'black'
            this.myCtx.fillRect(0, 0, w, h)
        },
        _drawBars: function (xPos, barHeight, barWidth) {
            this.myCtx.fillStyle = 'white'
            var yPos = (this.myCanvas.height - barHeight)
            this.myCtx.fillRect(xPos, yPos, barWidth, barHeight)
        }
    }
 }
</script>

<style scoped>
    .visualizer {
        width: 550px;
        height: 300px;
        background-color: black;
        border-radius: 17px;
    }
</style>