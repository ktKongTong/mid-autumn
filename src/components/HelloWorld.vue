<script setup>
import { onMounted, ref } from 'vue'
import { SVG } from '@svgdotjs/svg.js'

defineProps({
  msg: String
})

const count = ref(0)
onMounted(()=>{
  let width = document.body.clientWidth
  let height = document.body.clientHeight
  
  // console.log()
  // let width = 192
  // let height = 108
  const draw = SVG().addTo('#moon').size(width,height)
  const group = draw.group()
  let starGroup = []
  // 星星
  for(let i of new Array(500)){
    let item = draw.circle(2).attr({ 
      cx: Math.floor((Math.random()*width)+1),
      cy: Math.floor((Math.random()*height)+1),
      fill:'#b5bcc6'
      })
    starGroup.push(item)
    group.add(item)
  }
  // 渐变色
  let gradient = draw.gradient('linear', function(add) {
  add.stop(0, '#f9cc9d')
  add.stop(1, '#f7d50f')
  })
  let r = 200
  if(width<800){
    r=100
  }
  const moon = draw.circle(r).attr({ fill: gradient,cx:width-r,cy:r  })
  // moon.
  group.add(moon)




  
  // 流星
  // 流星渐变
  let linegrad = draw.gradient('linear', function(add) {
    add.stop({offset:0, color:'#ffffff',opacity:1})
    add.stop({offset:1, color:'#b5bcc6',opacity:0})
  })
  // 流星线
  let linePosArray = [
    {pos:[width+200, -height, width, -height+200],endpos:[-200,height/3]},
    {pos:[width/2+300, -height, width/2, -height+300],endpos:[-300,height/2]},
    {pos:[width+700, -height+500, width+500, -height+700],endpos:[-500,height]},
    ]
  for(let array of linePosArray){
    let line = draw.line(...array['pos'])
    line.stroke({ color: linegrad, width: 2, linecap: 'round' })
    line.animate({
    duration: 1500,
    delay: Math.floor(Math.random() * (6000 - 1800)) + 1800,
    when: 'now',
    times: Number.MAX_SAFE_INTEGER,
    wait: Math.floor(Math.random() * (6000 - 1800)) + 1800
    }).move(...array['endpos'])
    group.add(line)
  }
  let text = draw.text("中秋节快乐🥳🥳🥳").font({size:30, fill: '#b5bcc6', family: 'Microsoft YaHei' })
  console.log(text.length())
  text.move(width/2-text.length()/2,height-100)
  group.add(text)


  // 窗口尺寸改变
  window.onresize =(event)=>{
    let width = document.body.clientWidth
    let height = document.body.clientHeight
    draw.size(width,height)
    moon.attr({cx:width-r,cy:r})
    text.attr({x:width/2-text.length()/2,y:height-100})
    for(let item of starGroup){
      item.attr({
        cx: Math.floor((Math.random()*width)+1),
        cy: Math.floor((Math.random()*height)+1),
      })
    }
  }
})


</script>

<template>
  <div id="moon">

  </div>
</template>

<style scoped>
  #moon{
    width: 100vw;
    height: 100%;
  }
</style>
