---
published: true
title: "React에서 canvas로 그리기 2"

categories:
  # pick one
  - React
tags:
  - [TIL, devlog]

toc: true
toc_sticky: true

date: 2022-04-04
last_modified_at: 2022-04-04
---

# [React] canvas로 그리기 2

#### class 로 리팩토링

[canvas로 그리기 1](https://wonjaeji0801.github.io/react/canvas-in-React-1/)에서 `beginPath` 를 사용해 path 가 그려지는 시점을 정함으로써 여러개의 arc를 구분했습니다.

arc 외에도 여러 개의 primitive를 추가하거나 여러 기능(이동 및 애니메이션, 숨기기 등)을 bind하려면, class 형태로 독립적으로 관리할 필요가 있습니다.

```js
class Circle {
  constructor(ctx, coord, r, color) {
    this.ctx = ctx;
    this.coord = coord;
    this.r = r;
    this.color = color;
  }

  draw() {
    this.ctx.fillStyle = this.color;
    this.ctx.beginPath();
    this.ctx.arc(this.coord.x, this.coord.y, this.r, 0, 2 * Math.PI);
    this.ctx.fill();
    this.ctx.stroke();
  }
}

class Grid {
  constructor(ctx, dimensions, lineWidth, strokeStyle, density) {
    this.ctx = ctx;
    this.dimensions = dimensions;
    this.lineWidth = lineWidth;
    this.strokeStyle = strokeStyle;
    this.density = density;
  }

  draw() {
    this.ctx.lineWidth = this.lineWidth;
    this.ctx.strokeStyle = this.strokeStyle;
    this.ctx.beginPath();
    this.ctx.moveTo(0, this.dimensions.height / 2);
    this.ctx.lineTo(this.dimensions.width, this.dimensions.height / 2);
    this.ctx.stroke();
  }

  hide() {
    this.ctx.strokeStyle = "none";
  }
}
```

- `Circle` 과 `Grid` class

```js
export default function Canvas({ radius, showGrid }) {
  const canvasRef = useRef(null);

  const dimensions = {
    width: window.innerWidth * 0.7,
    height: window.innerHeight * 0.7,
  };

  function drawMain() {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");
    canvas.width = dimensions.width;
    canvas.height = dimensions.height;

    ctx.globalAlpha = 0.5;

    const blueCircle = new Circle(ctx, { x: 500, y: 150 }, radius, "skyblue");
    const redCircle = new Circle(ctx, { x: 400, y: 450 }, radius, "coral");
    const yellowCircle = new Circle(ctx, { x: 200, y: 350 }, radius, "yellow");

    const exGrid = new Grid(ctx, dimensions, 2, "blue", 0);

    blueCircle.draw();
    redCircle.draw();
    yellowCircle.draw();

    if (showGrid) {
      exGrid.draw();
    } else {
      exGrid.hide();
    }
  }

  useEffect(() => {
    drawMain();
  }, [radius, showGrid]);

  return (
    <div className="canvas">
      <canvas ref={canvasRef}></canvas>
    </div>
  );
}
```

마찬가지로 `useEffect` 안에서 `ctx`를 정의하고, `radius`, `showGrid` 상태를 두 번째 인자로 받습니다. 각 상태에 따라 동적으로 `draw`, `hide` 메서드를 호출합니다.

<br/>

![canvas_in_React_2](/assets/images/canvas_in_React_2.png)

<br/>
<br/>
<br/>

[맨 위로](#)
