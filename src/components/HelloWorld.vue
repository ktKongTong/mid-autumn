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
  let path1 = draw.path('M8,25c0-4.418,3.582-8,8-8 s8,3.582,8,8')
  let path2 = draw.path('M50.003,27 c0,0-2.535-0.375-5.003,0')
  let path3 = draw.path('M50.003,27 c-0.115-8.699-7.193-16-15.919-16c-5.559,0-10.779,3.005-13.661,7.336C19.157,17.493,17.636,17,16,17c-4.418,0-8,3.582-8,8 c0,0.153,0.014,0.302,0.023,0.454C8.013,25.636,8,25.82,8,26c-3.988,1.912-7,6.457-7,11.155C1,43.67,6.33,49,12.845,49h24.507 c0.138,0,0.272-0.016,0.408-0.021C37.897,48.984,38.031,49,38.169,49h9.803C54.037,49,59,44.037,59,37.972 C59,32.601,55.106,27.961,50.003,27z')
  cloudGroup.add(path1)
  cloudGroup.add(path2)
  cloudGroup.add(path3)
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
