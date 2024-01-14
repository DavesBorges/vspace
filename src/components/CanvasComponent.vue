<template>
  <svg></svg>
</template>

<script setup lang="ts">
import * as d3 from 'd3';
import { onMounted, watchEffect } from 'vue';

const props = defineProps<{ data: [number, number][] }>();

onMounted(() => {
  const canvasWidth = 640;
  const canvasHeight = 400;
  const margintTop = 20;
  const marginRight = 20;
  const marginBottom = 20;
  const marginLeft = 20;
  const graphWidth = 640 - marginLeft - marginRight;
  const graphHeight = 400 - margintTop - marginBottom;

  const x = d3.scaleLinear().domain([-5, 5]).range([marginLeft, graphWidth]);

  const line = d3
    .line()
    .x((d) => x(d[0]))
    .y((d) => y(d[1]));
  const y = d3.scaleLinear().domain([-5, 5]).range([graphHeight, marginBottom]);

  const svg = d3.select('svg').attr('width', canvasWidth).attr('height', canvasHeight);

  //Add the x-axis
  svg
    .append('g')
    .attr('transform', `translate(0, ${(canvasHeight - marginBottom) / 2})`)
    .call(d3.axisBottom(x));

  svg
    .append('g')
    .attr('transform', `translate(${(canvasWidth - marginRight) / 2})`)
    .call(d3.axisLeft(y));

  svg
    .selectAll('.tick')
    .filter((d) => d === 0)
    .remove();

  svg
    .append('text')
    .attr('text-anchor', 'end')
    .attr('fill', 'gray')
    .attr('x', x(0))
    .attr('y', y(0))
    .attr('transform', 'translate(-5, 18)')
    .text('0');

  watchEffect(() => {
    svg.selectAll('.line').remove();

    const vectors = props.data.map((entry) => [[0, 0], entry] as [number, number][]);

    vectors.forEach((vector) => {
      svg
        .append('defs')
        .append('marker')
        .attr('id', 'arrow')
        .attr('orient', 'auto-start-reverse')
        .attr('view-box', [0, 0, 10, 10])
        .attr('markerWidth', '10')
        .attr('markerHeight', '10')
        .attr('refX', '12')
        .attr('refY', '5')
        .append('path')
        .attr('d', 'M 0 0 L 10 5 L 0 10 z')
        .attr('stroke', 'steelblue')
        .attr('fill', 'steelblue');
      svg
        .selectAll('.line')
        .data(props.data)
        .join('g')
        .attr('class', 'line')
        .append('path')
        .attr('fill', 'none')
        .attr('stroke', 'steelblue')
        .attr('marker-end', 'url(#arrow)')
        .attr('d', line(vector));
    });

    props.data.forEach((entry) => {
      // x = a line
      const verticalLinePts = [[entry[0], 0], entry] as [number, number][];
      svg
        .append('path')
        .attr('class', 'line')
        .style('stroke-dasharray', '3, 3')
        .attr('stroke', 'steelblue')
        .attr('d', line(verticalLinePts));

      // y = a line
      const horizontalLinePts = [[0, entry[1]], entry] as [number, number][];
      svg
        .append('path')
        .attr('class', 'line')
        .attr('stroke', 'steelblue')
        .style('stroke-dasharray', '3, 3')
        .attr('d', line(horizontalLinePts));
    });
  });
});
</script>

<style scoped></style>
