<script setup>
import { onMounted, ref } from 'vue'
import '@svgdotjs/svg.filter.js';
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
      if(Math.random()>0.95){
        item.animate({
          duration: 1500,
          delay: 0,
          when: 'now',
          times: Number.MAX_SAFE_INTEGER,
          wait: Math.floor(Math.random() * (16000 - 8000)) + 8000
        }).css({visibility:'hidden'})
      }
    starGroup.push(item)
    group.add(item)
  }
  // 渐变色
  let gradient = draw.gradient('linear', function(add) {
  add.stop(0, '#fffc99')
  add.stop(1, '#ffe71d')
  })
  // 边缘渐变
  let bordergradient = draw.gradient('radial', function(add) {
  add.stop(0, '#b5bcc6')
  add.stop(1, '#fee71d')
})

  let r = 200
  if(width<800){
    r=100
  }
  const moon = draw.circle(r).attr({ fill: gradient,cx:width-r,cy:r  })
  // moon.
  moon.stroke({ color:bordergradient, width: 3})
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
  // console.log(text.length())
  text.move(width/2-text.length()/2,height-100)
  group.add(text)

  // 云组
  const cloudGroup = draw.group()
  let cloud = draw.path('M43.36,16.67a9.4,9.4,0,0,0,.23-2.05A8.8,8.8,0,0,0,30,7.16,14.61,14.61,0,0,0,2.87,14.68a14.14,14.14,0,0,0,.25,2.68,6.47,6.47,0,0,0,3.31,12h35.3a6.46,6.46,0,0,0,1.63-12.69Z')
  let smile = draw.path('M18.85,22.49a2.32,2.32,0,0,1-.46,0,2.21,2.21,0,0,1-1.51-1.17.31.31,0,0,1,.56-.28,1.58,1.58,0,0,0,2.48.45.31.31,0,0,1,.44,0,.32.32,0,0,1,0,.45A2.2,2.2,0,0,1,18.85,22.49Z')
  smile.attr({fill:'#7FABDA'})
  let cloudEye1 = draw.ellipse(1.75,1.76).attr({fill:'#000',cx:11.92,cy:16.26})
  let eyeWhite1 = draw.circle(0.65).attr({fill:'#7FABDA',cx:12.39,cy:15.83})
  let cloudEye2 = draw.ellipse(1.75,1.76).attr({fill:'#000',cx:26.54,cy:18.03})
  let eyeWhite2 = draw.circle(0.65).attr({fill:'#7FABDA',cx:27.01,cy:17.6})
  cloudGroup.add(cloud)
  cloudGroup.add(smile)
  cloudGroup.add(cloudEye1)
  cloudGroup.add(eyeWhite1)
  cloudGroup.add(cloudEye2)
  cloudGroup.add(eyeWhite2)
  cloudGroup.css({
    'fill':'#7FABDA',
    'stroke':'#7383BF',
    'stroke-width':0,
    'stroke-linecap':'round',
    'stroke-miterlimit':10
  })
  cloudGroup.move(width-r,r+20)
  cloudGroup.css({opacity:0.8})
  cloudGroup.animate({
    duration: 5500,
    delay: 1000,
    when: 'now',
    swing:true,
    times: Number.MAX_SAFE_INTEGER,
    // wait: Math.floor(Math.random() * (6000 - 1800)) + 1800
  }).move(width-r-100,r+20)
  if(width>800){
      cloudGroup.scale(2)
  }

  group.add(cloudGroup)
  // 窗口尺寸改变
  window.onresize =(event)=>{
    let width = document.body.clientWidth
    let height = document.body.clientHeight
    draw.size(width,height)
    moon.attr({cx:width-r,cy:r})
    cloudGroup.move(width-r,r)
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
