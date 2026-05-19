---
title: "Recharts"
date: 2026-05-18
tags:
  - wiki
  - programming/frontend
  - programming/visualization
aliases:
  - "Recharts Library"
---

# Recharts

## Table of Contents
- [[#Simple Explanation]]
- [[#Mental Model]]
- [[#Basic Anatomy Of A Recharts Chart]]
- [[#Example Bar Chart]]
- [[#Example Pie Chart]]
- [[#Component Catalogue]]
- [[#Chart Containers And Layout]]
- [[#Cartesian Chart Types]]
- [[#Polar Chart Types]]
- [[#Series Components]]
- [[#Axes Structure And Guides]]
- [[#Interaction Labels And Styling]]
- [[#Choosing The Right Chart]]
- [[#How To Use It Better]]
- [[#How We Use It In Canvas]]
- [[#Common Mistakes]]
- [[#Related Notes]]

## Simple Explanation

Recharts is the charting library we use for data visualization in React. It turns structured data into bars, lines, pies, axes, legends, tooltips, and responsive dashboard visuals.

The important idea is that Recharts is a composition system, not one giant chart component. You build a chart by combining a container, a chart type, one or more series, and the supporting pieces like axes, grids, labels, and tooltips.

## Mental Model

Most Recharts code follows the same path:

1. Start with an array of objects
2. Choose the chart type that matches the question you are answering
3. Map keys from the data into visual series
4. Add structural helpers like axes, grid, legend, and tooltip
5. Wrap the chart in `ResponsiveContainer` so it resizes correctly inside the layout

If the data shape is clean, Recharts feels simple. If the data shape is inconsistent, the chart code gets noisy fast.

## Basic Anatomy Of A Recharts Chart

```tsx
<ResponsiveContainer width="100%" height={320}>
  <BarChart data={data}>
    <CartesianGrid strokeDasharray="3 3" />
    <XAxis dataKey="month" />
    <YAxis />
    <Tooltip />
    <Legend />
    <Bar dataKey="revenue" fill="#0f766e" />
  </BarChart>
</ResponsiveContainer>
```

Each part has a job:

- `ResponsiveContainer` gives the chart a responsive box to render inside
- `BarChart` defines the overall chart type
- `XAxis` and `YAxis` define how values are interpreted visually
- `Tooltip` and `Legend` help the user understand the chart
- `Bar` is the data series being drawn

That same pattern repeats across most Recharts chart families.

## Example Bar Chart

```tsx
const monthlyData = [
  { month: 'Jan', revenue: 42000, target: 38000 },
  { month: 'Feb', revenue: 39000, target: 40000 },
  { month: 'Mar', revenue: 47000, target: 41000 },
]

<ResponsiveContainer width="100%" height={320}>
  <BarChart
    data={monthlyData}
    margin={{ top: 8, right: 16, left: 0, bottom: 0 }}
  >
    <CartesianGrid strokeDasharray="3 3" />
    <XAxis dataKey="month" />
    <YAxis tickFormatter={(value) => `$${value / 1000}k`} />
    <Tooltip formatter={(value) => [`$${value.toLocaleString()}`, 'Revenue']} />
    <Legend />
    <Bar dataKey="revenue" fill="#0f766e" radius={[6, 6, 0, 0]} />
    <Bar dataKey="target" fill="#94a3b8" radius={[6, 6, 0, 0]} />
  </BarChart>
</ResponsiveContainer>
```

Use this pattern when you want category comparison:

- revenue by month
- issues by team
- tickets by status
- submissions by workflow stage

Bar charts are usually the best default when the goal is comparison across categories.

## Example Pie Chart

```tsx
const statusData = [
  { name: 'Complete', value: 62 },
  { name: 'In Progress', value: 24 },
  { name: 'Blocked', value: 14 },
]

const colors = ['#0f766e', '#f59e0b', '#dc2626']

<ResponsiveContainer width="100%" height={320}>
  <PieChart>
    <Pie
      data={statusData}
      dataKey="value"
      nameKey="name"
      cx="50%"
      cy="50%"
      outerRadius={100}
      label
    >
      {statusData.map((entry, index) => (
        <Cell key={entry.name} fill={colors[index % colors.length]} />
      ))}
    </Pie>
    <Tooltip />
    <Legend />
  </PieChart>
</ResponsiveContainer>
```

Use this when you want to show share of a whole. Avoid it when people need to compare values precisely; bars are usually better for that.

## Component Catalogue

Recharts components make more sense when grouped by role instead of memorized one by one.

## Chart Containers And Layout

- **`ResponsiveContainer`** wraps the chart so it fills the available width and respects a fixed height
- **`ComposedChart`** lets you mix bars, lines, and areas in one chart when one series type is not enough

If you are building dashboard widgets, `ResponsiveContainer` should be your default. Most layout bugs happen because the parent has no height.

## Cartesian Chart Types

- **`BarChart`** compares categories with bars
- **`LineChart`** shows trends over time or ordered sequences
- **`AreaChart`** emphasizes trend plus total volume under the line
- **`ComposedChart`** mixes bars, lines, and areas in one coordinate space
- **`ScatterChart`** shows relationships between numeric dimensions

These are the chart types you use most often for dashboards, reports, and metrics screens.

## Polar Chart Types

- **`PieChart`** shows proportion of a whole
- **`RadarChart`** compares several dimensions in a radial layout
- **`RadialBarChart`** shows circular progress or grouped radial metrics

Polar charts are visually strong but easier to misuse. Use them when the radial shape actually helps the reader.

## Series Components

- **`Bar`** draws one bar series inside a `BarChart` or `ComposedChart`
- **`Line`** draws one line series
- **`Area`** draws one filled area series
- **`Pie`** draws one pie or donut series
- **`Scatter`** draws point-based series
- **`Radar`** draws one radar series
- **`RadialBar`** draws one circular bar series

Series components are where data keys become actual visuals.

## Axes Structure And Guides

- **`XAxis`** defines horizontal categories or values
- **`YAxis`** defines vertical scale and tick formatting
- **`ZAxis`** is used when a third numeric dimension matters in scatter-style charts
- **`CartesianGrid`** adds reference lines for easier reading
- **`ReferenceLine`** marks thresholds, targets, or baselines
- **`ReferenceArea`** highlights a range or region in the chart
- **`ReferenceDot`** highlights one important point
- **`Brush`** lets users zoom into a subsection of a longer chart

These components do not change the underlying data, but they strongly affect readability.

## Interaction Labels And Styling

- **`Tooltip`** shows detail on hover
- **`Legend`** explains colors and series names
- **`Label`** adds a single label to an axis or chart element
- **`LabelList`** adds repeated labels to bars, points, or slices
- **`Cell`** styles individual bars or pie slices one by one

This category is where charts become understandable instead of merely decorative.

## Choosing The Right Chart

- Use **`BarChart`** when comparing categories side by side
- Use **`LineChart`** when tracking change over time
- Use **`AreaChart`** when cumulative visual weight matters as much as trend
- Use **`PieChart`** only for simple proportions with a small number of segments
- Use **`ComposedChart`** when one series should be bars and another should be a line
- Use **`ScatterChart`** when the relationship between numeric variables matters more than category comparison

In most product dashboards, bar charts and line charts do most of the useful work.

## How To Use It Better

- Normalize data before rendering so every chart gets a predictable shape
- Keep formatting helpers shared and pure, especially for currency, percentages, and abbreviations
- Prefer wrapper components for repeated styling, spacing, tooltip behavior, and legends
- Keep charts presentational; fetch and transform data outside the chart component when possible
- Use a small number of colors and keep their meaning consistent across the app
- Make labels and units obvious so the chart can be read without a spoken explanation
- Use `Cell` only when per-point styling actually carries meaning
- Choose the smallest set of chart features that answers the question clearly

Recharts gets easier to maintain when each chart solves one question cleanly instead of trying to show everything at once.

## How We Use It In Canvas

Canvas currently has a small chart layer under `src/components/charts/`:

- `BarChart.jsx`
- `GroupedBarChart.jsx`
- `PieChart.jsx`

The implementation pattern is already fairly consistent:

- charts are client components with `'use client'`
- charts are wrapped in `ResponsiveContainer`
- bar-style charts import `BarChart`, `Bar`, `XAxis`, `YAxis`, `CartesianGrid`, `Tooltip`, `Legend`, and `ResponsiveContainer`
- pie charts import `PieChart`, `Pie`, `Cell`, `Tooltip`, and `ResponsiveContainer`
- custom tooltip and legend components are used for shared styling
- helper functions format large numbers and tooltip values

The current prop shapes are moving toward reusable chart wrappers:

```tsx
// Bar-style chart wrapper
{
  data,
  keys,
  xKey = 'month',
  height = 320,
  format = 'number',
}

// Pie-style chart wrapper
{
  data,
  height = 320,
  title,
}
```

That is a good direction because it keeps page code focused on data while the chart components own rendering, formatting, and layout concerns.

## Common Mistakes

- The parent container has no height, so the chart renders blank even though the JSX is valid
- The `dataKey` does not match the real object shape, so the series silently disappears
- Too many series are packed into one chart, making the legend and colors meaningless
- Pie charts are used for precise comparison when bars would be easier to read
- Tooltip and axis formatters do too much work inside render and make the component noisy
- Data transformation happens inline inside JSX instead of before render
- In Next.js, the chart is treated like a server component even though the library needs client-side rendering

If a Recharts component feels hard to maintain, the root cause is usually poor data shape or too much responsibility in one chart.

## Related Notes

- [[Canvas]] — the app where these charts live
- [[Canvas React Components]] — the component inventory for the Canvas app
- [[React]] — the UI layer Recharts renders into
- [[MOC - Programming]] — the programming index for this vault
