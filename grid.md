# Grids in CSS

## Creating a Grid
To create a grid you need a container and items inside of it.

The container must get a css rule `display: grid;`.

To create columns we can use `grid-template-columns` rule on the grid container; we then specify sizes of wanted columns like this: `grid-template-columns: 100px 50% 200px;`. This means that we create 3 columns, first will be 100px wide, second will be 50% of the grid width wide and third will be 200px wide.

The same goes to rows, but we will use `grid-template-rows` instead.

We can also achieve the same with one property: `grid-template`. Like so:
```
grid-template: 40% 50% 50px / 100px 50% 200px;
```

When you are using `grid-template` you must specift rows first, columns come second.

We can also use fractions. When using them, we define the size of columns and rows as a fraction of the grid's length and width. For example if we use `grid-template-rows: 1fr fr 1fr`, there will be 3 rows, and their height will be divided by four. First row will have one fourth of the grid height, second row will have two fourths of the heigth and so on.

We can also use `repeat()` function. It will create _n_ number of _m_ size rows/columns if we specify like this: `grid-template-rows: repeate(n, m)`.

For resizable grids we can add `minmax()` function as a value of the row/column size. It takes two values of size as parameters.

We can have gaps between rows and columns by using `grid-column-gap` and `grid-row-gap` properties. We can also use only one for both: `grid-gap`.


## Grid items
Grid consists of items. 
We can choose where the grid item will be placed by specifying these parameters: `grid-row-start` and `grid-row-end`. It will tell how many rows will the item occupy. 

We can also achieve the same by running `grid-row: n / m`, where _n_ is the starting row and _m_ is the ending row of the grid item.

We can apply the same rules to the column positioning.

We can also use keyword `span` to specify `how many` rows/columns we wan't to occupy. For example:

```
grid-column: 2 / 4;

```

and

```
grid-column: 2 / span 2;

```

Will do the same thing.

We can also combine all of those properties into one with `grid-area`. But the order of the parameters is important. It goes like this:
1. `grid-row-start`
2. `grid-column-start`
3. `grid-row-end`
4. `grid-column-end`


## Grid Template Areas
The `grid-template-areas` property allows you to name sections of your web page to use as values in the `grid-row-start`, `grid-row-end`, `grid-col-start`, `grid-col-end` and `grid-area properties`.

So we need to provide the container with a site structure. Example:
```

.container {
  display: grid;
  max-width: 900px;
  position: relative;
  margin: auto;
  grid-gap: 10px;
  grid-template-areas: 
    "header header"
    "nav nav"
    "left right"
    "footer footer";
}

```

And the we can just use the `grid-area` property on grid-items, like this:
```
.header {
  grid-area: header;
}
```

## Justify Items
`justify-items` is a property that positions grid items along the inline, or row, axis. It accepts values:
- `start`
- `end`
- `center`
- `stretch` - stretches all items to fill the grid area

`justify-content` is a property that is used to position the entire grid along the row axis. It accpets values:
- `start`
- `end`
- `center`
- `stretch`
- `space-around`
- `space-between`
- `space-evenly`