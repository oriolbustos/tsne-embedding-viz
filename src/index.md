```html
<div style="display: flex; width: 100%; justify-content: center; flex-direction: column; align-items: center; text-align: center;">
  <h1>t-distributed stochastic neighbor embedding (t-SNE)</h1>
  <p class="subtitle"><em>Visualization of real vs. generated blood glucose data</em></p>
  <p>Blood glucose data was converted into 3 dimensions using a delayed embedding. After that, it was transformed with a 3-dimensional t-SNE.</p>
</div>


<div id="myDiv" style="width: 100%; height: 600px; display: flex; justify-content: center; align-items: center;"></div>

<div style="display: flex; justify-content: center; align-items: center; margin-top: 2px;">
  <div style="display: flex; align-items: center; margin-right: 20px;">
    <div style="width: 15px; height: 15px; background-color: red; margin-right: 5px; border-radius: 50%;"></div>
    <span>Generated Blood Glucose Data</span>
  </div>
  <div style="display: flex; align-items: center;">
    <div style="width: 15px; height: 15px; background-color: blue; margin-right: 5px; border-radius: 50%;"></div>
    <span>Real Blood Glucose Data</span>
  </div>
</div>

```

---

```html
<div style="display: flex; width: 100%; justify-content: center; flex-direction: column; align-items: center; text-align: center;">
  <h2>t-SNE Coloured with Blood Glucose Values</h2>
  <p class="subtitle"><em>Visualization of generated blood glucose data</em></p>
    <p>Blood glucose data is visualized here in colour. It can be seen how it is distributed along the first component.</p>

</div>
<div id="myDiv2" style="width:100%; height:600px;"></div>
```


```js
import Plotly from "plotly.js-dist-min"   // ← this *declares* Plotly
```

```js
const tsne3d = FileAttachment("tsne_3d_results-4.csv").csv({typed: true})
```

```js
const realData = tsne3d.filter(d => d.Type === "Real")
```

```js
const generatedData = tsne3d.filter(d => d.Type === "Generated");
```

```js
const tsne_3d_viz = Plotly.newPlot('myDiv', {
  data: [
    {
      x: realData.map(d => d.tsne_1*2),
      y: realData.map(d => d.tsne_2*2),
      z: realData.map(d => d.tsne_3*2),
      mode: 'markers',
      type: 'scatter3d',
      marker: { color: 'blue', size: 3 },
      name: 'Real Blood Glucose Data',
      opacity: 0.7,
      showlegend: false
    },
    {
      x: generatedData.map(d => d.tsne_1*2),
      y: generatedData.map(d => d.tsne_2*2),
      z: generatedData.map(d => d.tsne_3*2),
      mode: 'markers',
      type: 'scatter3d',
      marker: { color: 'red', size: 3 },
      name: 'Generated Blood Glucose Data',
      opacity: 0.7,
      showlegend: false
    },
  ],
    layout: {
    scene: {
      xaxis: { title: { text: 't-SNE 1' } },
      yaxis: { title: { text: 't-SNE 2' } },
      zaxis: { title: { text: 't-SNE 3' } }
    }
  },
});

```

```js
const colors = realData.map(d => d.BG)
```

```js
const colors_gen = generatedData.map(d => d.BG)
```

```js
Plotly.newPlot('myDiv2', {
  data: [
    {
      x: realData.map(d => d.tsne_1),
      y: realData.map(d => d.tsne_2),
      z: realData.map(d => d.tsne_3),
      mode: 'markers',
      type: 'scatter3d',
      marker: {
        size: 3,
        color: colors, // Use the BG values as the color scale
        colorscale: 'Viridis', // Choose a colorscale (e.g., 'Viridis', 'Plasma', etc.)
        colorbar: {
          title: 'BG Value', // Add a colorbar title
        },
      },
      name: 'Real',
    },
  ],
  layout: {
    scene: {
      xaxis: { title: { text: 't-SNE 1' } },
      yaxis: { title: { text: 't-SNE 2' } },
      zaxis: { title: { text: 't-SNE 3' } }
    }
  },
});

```
