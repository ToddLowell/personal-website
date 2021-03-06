<template>
  <Layout img="/generativeArt/heading.png" title="Generative Art" date="December 2020">
    <div slot="content">
      <blockquote>
        <p>Pssst. Refresh to page to re-generate the images below.</p>
      </blockquote>
      <p>
        When starting a project without a specific goal in mind it can help to first pick a simple theme and keep
        abstracting from there. Here we start with a simple grid.
      </p>
      <canvas id="c0" />
      <p>
        But straight lines are boring so let's change the intersections of the grid to circles and see where we can go
        from there.
      </p>
      <canvas id="c1" />
      <p>
        In tune with the title of this page, "Generative Art", it's time to add some randomness! Let's make the circles
        smaller and randomly show half of them with <AppPrism lang="javascript">Math.random() > 0.5</AppPrism>.
      </p>
      <canvas id="c2" />
      <p>
        Now let's also make the sizes of the circles random too! But instead of using the usual
        <AppPrism lang="javascript">Math.random()</AppPrism> which gives us a random number from 0 (inclusive) to 1
        (exclusive) we will use the Gaussuan (or Normal) distribution. Gaussian distributions show up very often in
        nature and will give us a more organic and natural feeling randomness.
      </p>
      <canvas id="c3" />
      <p>
        And lastly let's add color! nice-color-palettes is a npm package with color paletters sourced from
        <AppLink href="https://www.colourlovers.com/" target="_blank" rel="noopener noreferrer">
          ColourLovers.com
        </AppLink>
        to give us nicely matching colors.
      </p>
      <canvas id="c4" />
      <AppButton class="center" @click="randomColor">Click Me!</AppButton>
      <p>
        From a simple grid of circles we can create abstract computer generated art by simply adding randomness as much
        as possible (within reason of course). From randomly showing circles to random sizes to random colors the end
        product looks quite nice.
      </p>
      <p>
        While HTML5 Canvas is very performant, one con is that it is a single element so using it for web interactivity
        is difficult. An alternative is SVGs where each shape is its own element. Using
        <AppLink href="https://d3js.org/" target="_blank" rel="noopener noreferrer">D3.js</AppLink> we can make what we
        have above but with SVGs. We can also make it interactive by giving it a hover effect. Give it a try!
      </p>
      <svg ref="svg" />
      <AppButton class="center" @click="drawSVG">Click Me!</AppButton>
    </div>
  </Layout>
</template>

<script>
import palettes from 'nice-color-palettes';
import _ from 'lodash';
import * as d3 from 'd3';

import Layout from '~/layout/blog.vue';

export default {
  layout: 'empty',

  components: {
    Layout,
  },

  mounted() {
    this.grid();
    this.circleGrid();
    this.randomDisplay();
    this.randomSizes();
    this.randomColor();
    this.drawSVG();
  },

  methods: {
    lerp(min, max, t) {
      return min * (1 - t) + max * t;
    },

    grid() {
      const canvas0 = document.querySelector('canvas#c0');
      const width = 2048;
      const height = 2048;

      canvas0.width = width;
      canvas0.height = height;

      const createGrid = () => {
        const points = [];
        const count = 5;
        for (let x = 0; x < count; x++) {
          for (let y = 0; y < count; y++) {
            const u = count <= 1 ? 0.5 : x / (count - 1);
            const v = count <= 1 ? 0.5 : y / (count - 1);

            points.push([u, v]);
          }
        }

        return points;
      };

      const points = createGrid();
      const margin = 400;

      const ctx = canvas0.getContext('2d');
      points.forEach(([u, v]) => {
        // const x = u * width;
        // const y = v * height;
        const x = this.lerp(margin, width - margin, u);
        const y = this.lerp(margin, width - margin, v);

        ctx.strokeStyle = '#ddd';
        ctx.lineWidth = 30;

        ctx.moveTo(margin / 2, y);
        ctx.lineTo(width - margin / 2, y);
        ctx.stroke();

        ctx.moveTo(x, margin / 2);
        ctx.lineTo(x, height - margin / 2);
        ctx.stroke();
      });
    },

    circleGrid() {
      const canvas1 = document.querySelector('canvas#c1');
      const width = 2048;
      const height = 2048;

      canvas1.width = width;
      canvas1.height = height;

      const createGrid = () => {
        const points = [];
        const count = 5;
        for (let x = 0; x < count; x++) {
          for (let y = 0; y < count; y++) {
            const u = count <= 1 ? 0.5 : x / (count - 1);
            const v = count <= 1 ? 0.5 : y / (count - 1);

            points.push([u, v]);
          }
        }

        return points;
      };

      const points = createGrid();
      const margin = 400;

      const ctx = canvas1.getContext('2d');
      points.forEach(([u, v]) => {
        const x = this.lerp(margin, width - margin, u);
        const y = this.lerp(margin, width - margin, v);

        ctx.beginPath();
        ctx.arc(x, y, 100, 0, Math.PI * 2, false);
        ctx.strokeStyle = '#ddd';
        ctx.lineWidth = 30;
        ctx.stroke();
      });
    },

    randomDisplay() {
      const canvas2 = document.querySelector('canvas#c2');
      const width = 2048;
      const height = 2048;

      canvas2.width = width;
      canvas2.height = height;

      const createGrid = () => {
        const points = [];
        const count = 40;
        for (let x = 0; x < count; x++) {
          for (let y = 0; y < count; y++) {
            const u = count <= 1 ? 0.5 : x / (count - 1);
            const v = count <= 1 ? 0.5 : y / (count - 1);

            points.push([u, v]);
          }
        }

        return points;
      };

      const points = createGrid().filter(() => Math.random() > 0.5);
      const margin = 100;

      const ctx = canvas2.getContext('2d');
      points.forEach(([u, v]) => {
        const x = this.lerp(margin, width - margin, u);
        const y = this.lerp(margin, width - margin, v);

        ctx.beginPath();
        ctx.arc(x, y, 5, 0, Math.PI * 2, false);
        ctx.strokeStyle = '#ddd';
        ctx.lineWidth = 20;
        ctx.stroke();
        ctx.fillStyle = '#ddd';
        ctx.fill();
      });
    },

    randomSizes() {
      const canvas3 = document.querySelector('canvas#c3');
      const width = 2048;
      const height = 2048;

      canvas3.width = width;
      canvas3.height = height;

      const createGrid = () => {
        const points = [];
        const count = 40;
        for (let x = 0; x < count; x++) {
          for (let y = 0; y < count; y++) {
            const u = count <= 1 ? 0.5 : x / (count - 1);
            const v = count <= 1 ? 0.5 : y / (count - 1);

            // const radius = Math.max(0, random.gaussian() * 0.01);
            const radius = d3.max([0, d3.randomNormal()() * 0.02]);

            points.push({
              position: [u, v],
              radius,
            });
          }
        }

        return points;
      };

      const points = createGrid().filter(() => Math.random() > 0.5);
      const margin = 100;

      const ctx = canvas3.getContext('2d');
      points.forEach((data) => {
        const {
          position: [u, v],
          radius,
        } = data;

        const x = this.lerp(margin, width - margin, u);
        const y = this.lerp(margin, width - margin, v);

        ctx.beginPath();
        ctx.arc(x, y, radius * width, 0, Math.PI * 2, false);
        ctx.fillStyle = '#ddd';
        ctx.fill();
      });
    },

    randomColor() {
      const canvas4 = document.querySelector('canvas#c4');

      const width = 2048;
      const height = 2048;

      canvas4.width = width;
      canvas4.height = height;

      const palette = _.chain(palettes).sample().shuffle().value();

      const createGrid = () => {
        const points = [];
        const count = 40;
        for (let x = 0; x < count; x++) {
          for (let y = 0; y < count; y++) {
            const u = count <= 1 ? 0.5 : x / (count - 1);
            const v = count <= 1 ? 0.5 : y / (count - 1);

            // const radius = Math.max(0, random.gaussian() * 0.01);
            const radius = d3.max([0, d3.randomNormal()() * 0.02]);

            points.push({
              color: _.sample(palette),
              position: [u, v],
              radius,
            });
          }
        }

        return points;
      };

      const points = createGrid().filter(() => Math.random() > 0.5);
      const margin = 100;

      const ctx = canvas4.getContext('2d');
      points.forEach((data) => {
        const {
          position: [u, v],
          radius,
          color,
        } = data;

        const x = this.lerp(margin, width - margin, u);
        const y = this.lerp(margin, width - margin, v);

        ctx.beginPath();
        ctx.arc(x, y, radius * width, 0, Math.PI * 2, false);
        ctx.fillStyle = color;
        ctx.fill();
      });
    },

    drawSVG() {
      const size = 2048;
      const margin = 50;
      const palette = _.chain(palettes).sample().shuffle().value();

      const createGrid = () => {
        const points = [];
        const count = 50;
        for (let x = 0; x < count; x++) {
          for (let y = 0; y < count; y++) {
            const u = count <= 1 ? 0.5 : x / (count - 1);
            const v = count <= 1 ? 0.5 : y / (count - 1);

            points.push({ x: u, y: v });
          }
        }

        const pointsRandom = _.filter(points, () => Math.random() > 0.5);
        const pointsClean = _.map(pointsRandom, (d) => {
          return {
            x: this.lerp(margin, size - margin, d.x),
            y: this.lerp(margin, size - margin, d.y),
            r: d3.max([0, d3.randomNormal()() * 40]),
            color: _.sample(palette),
          };
        });

        return pointsClean;
      };
      const points = createGrid();

      //  define transition
      const t = d3.transition().duration(750);

      d3.select(this.$refs.svg)
        .attr('width', '100%')
        .attr('height', '100%')
        .attr('viewBox', [0, 0, size, size])
        .attr('preserveAspectRatio', 'xMidYMid meet')
        .selectAll('circle')
        .data(points)
        .join(
          (enter) => {
            return enter
              .append('circle')
              .attr('cx', (d) => d.x)
              .attr('cy', (d) => d.y)
              .attr('r', (d) => 0);
          },
          (update) => update,
          (exit) => {
            exit.transition(t).attr('r', (d) => 0);
          }
        )
        .transition(t)
        .attr('r', (d) => d.r)
        .attr('fill', (d) => d.color);

      //  set hover logic
      const el = document.querySelectorAll('circle');

      for (let i = 0; i < el.length; i++) {
        el[i].addEventListener('mouseover', function () {
          this.setAttribute('style', 'fill:' + palette[Math.floor(Math.random() * palette.length)]);
        });
      }
    },
  },
};
</script>
