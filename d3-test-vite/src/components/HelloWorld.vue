<template>
  <div class="test-d3">
    <div class="d3-bar"></div>
  </div>
</template>

<script lang="ts" setup>
import * as d3 from "d3"
import d3Tip from 'd3-tip'
import {reactive,onMounted} from 'vue'
const  data= reactive([
        { letter: '一', frequency: 0.08167 },
        { letter: '二', frequency: 0.13492 },
        { letter: '三', frequency: 0.02782 },
        { letter: '四', frequency: 0.04253 },
        { letter: '五', frequency: 0.12702 },
        { letter: '六', frequency: 0.02288 },
        { letter: '日', frequency: 0.22288 }
      ])

  const  initChart =()=> {
      //画布周边空白
      const margin = {
        top: 80,
        right: 20,
        bottom: 30,
        left: 60
      }
      const width = 800 - margin.left - margin.right
      const height = 500 - margin.top - margin.bottom

      // 1.选择元素 添加svgds
      let chart = d3
        .selectAll('.test-d3')
        .select('.d3-bar')
        .append('svg')
        .attr('width', width + margin.left + margin.right)
        .attr('height', height + margin.top + margin.bottom)

      // 2.设置x轴和y轴坐标映射关系 比例尺 (scale) domain(定义域) range (值域)
      let max = d3.max(data, function (d) {
        return d.frequency
      })
      let xScale = d3.scaleBand().domain(data.map(function (d) {
        return d.letter
      })).range([0, width])
      let yScale = d3.scaleLinear().domain([0, max]).range([height, 0])


      //设置x轴和Y轴 axis
      let xAxis = d3.axisBottom(xScale)
      let yAxis = d3.axisLeft(yScale).ticks(10, '%')

      // 设置tip 
      let tip = d3Tip() // 设置tip
        .attr('class', 'd3-tip')
        .offset([-10, 0])
        .html(function (d) {
          console.log('d------', d)
          return (
            '<strong>星期' +
            d.target.__data__.letter +
            "<br>空置率:</strong> <span style='color:#ffeb3b'>" +
            (d.target.__data__.frequency * 100).toFixed(2) +
            '%</span>'
          )
        })
      chart.call(tip)

      let g = chart
        .append('g')
        .attr('transform', 'translate(' + margin.left + ',' + margin.top + ')') // 设最外包层在总图上的相对位置

      //绘制x轴 y轴
      g
        .append('g')
        .attr('class', 'axis axis-x')
        .attr('transform', 'translate(' + 0 + ',' + height + ')')
        .call(xAxis)
      g
        .append('g')
        .attr('class', 'axis axis-y')
        // .attr('transform', 'translate(40, -20)')
        .call(yAxis)
        .append('text')
        .style('text-anchor', 'middle')
        .style('fill', '#fff')
        .text('空置率 (%)')


      //画柱子x y坐标 移入移出柱子
      g.selectAll('.bar').data(data)
        .enter()
        .append('rect')
        .on('mouseover', tip.show)
        .on('mouseout', tip.hide)
        .attr('class', 'bar')
        .attr('x', function (d) {
          return xScale(d.letter)
        })
        .attr('y', function (d) {
          return yScale(d.frequency)
        })
        .attr('width', xScale.bandwidth() / 2)
        .attr('height', function (d) {
          return height - yScale(d.frequency)
        })
        .attr('transform', 'translate(' + xScale.bandwidth() / 4 + ',' + 0 + ')') // 设最外包层在总图上的相对位置
        .attr('fill', '#8a2be2')

      //输出图上数字
      g.append('g')
        .attr('transform', 'translate(' + xScale.bandwidth() / 5 + ',' + 5 + ')') // 设最外包层在总图上的相对位置
        .attr('class', 'bar-text')
        .selectAll('text')
        .data(data)
        .enter()
        .append('text')
        .attr('fill', '#ffffff')
        .attr('font-size', '14px')
        .attr('text-anchor', 'middle') //文本对齐方式
        .attr('x', function (d) {
          return xScale(d.letter)
        })
        .attr('y', function (d) {
          return yScale(d.frequency)
        })
        .attr('dx', 30)
        .attr('dy', '1em')
        .text(function (d) {
          return (d.frequency * 100).toFixed(2) + '%'
        })
        .on('mouseover', tip.show)
        .on('mouseout', tip.hide)
    }

  onMounted(() => {
    initChart() 
  })

</script>

<style>
.test-d3 {
  display: flex;
  justify-content: center;
  margin: auto;
}
.d3-tip {
  background: rgba(50, 50, 50, 0.7);
  border: 1px solid "#333";
  padding: 5px;
  border-radius: 4px;
  color: #ffffff;
  cursor: pointer;
}
</style>
