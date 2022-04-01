---
published: true
title: "React에서 canvas로 그리기 1"

categories:
  # pick one
  - React
tags:
  - [TIL, devlog]

toc: true
toc_sticky: true

date: 2022-03-31
last_modified_at: 2022-03-31
---

# [React] canvas 태그 사용법

#### canvas 태그

HTML 태그로, 캔버스 스크립팅 API 또는 WebGL API와 함께 사용해 웹 상에 렌더되는 그래픽과 애니메이션을 그릴 수 있습니다.

다음과 간단하고 선언적으로 primitive 도형을 그릴 수 있습니다.
getElementById 로 canvas DOM을 찾아, getContext 로 그리기 맥락을 설정하고, x = 150, y = 150 에 너비 100, 높이 100, 파란색 사각형이 그려지게 됩니다.

```js
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
ctx.fillStyle = "blue";
ctx.fillRect(150, 100, 100, 100);
```

```HTML
<canvas id = "canvas" width = "500" height = "500" style =' border : 1px solid #000 ';></canvas>
```

---

#### React

```js
function DrawSomething({ radius }) {
  const canvasRef = useRef(null);

  function drawThreeArc() {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");
    canvas.width = 1000;
    canvas.height = 1000;

    ctx.fillStyle = "skyblue";
    ctx.beginPath();
    // beginPath: 각 도형의 시작과 끝을 구분하기 위해 사용,
    // 없으면 도형 1의 끝과 도형 2의 시작 맥락이 이어짐
    ctx.arc(500, 150, radius, 0, 2 * Math.PI);
    ctx.fill();
    ctx.stroke();

    ctx.fillStyle = "lightyellow";
    ctx.beginPath();
    ctx.arc(200, 350, 100, 0, 2 * Math.PI);
    ctx.fill();
    ctx.stroke();

    ctx.fillStyle = "lightgreen";
    ctx.beginPath();
    ctx.arc(700, 450, 100, 0, 2 * Math.PI);
    ctx.fill();
    ctx.stroke();
  }

  useEffect(() => {
    drawThreeArc();
  }, [radius]);

  return (
    <div id="canvas" className="canvas">
      <canvas ref={canvasRef}></canvas>
    </div>
  );
}
```

1. useRef()를 사용하여 canvasRef라는 Ref 객체를 만들고, 해당 객체를 DOM으로 조작하고 싶은 canvas 태그의 Ref 값으로 설정합니다.
   <br/>
2. useEffect를 통해, 페이지가 렌더가 될때 canvas에 canvasRef라는 ref 객체의 .current라는 값을 할당합니다. (current 어트리뷰트에 담긴 캔버스 태그의 참조를 canvas에 할당하는 과정, 따라서 useEffect를 사용하지 않으면 canvasRef에 초기값 이후 할당되지 않아 TypeError가 발생합니다.)
   `Cannot read properties of null (reading 'getContext')`
   <br/>
3. 동적인 값(React state) radius을 useEffect 두 번째 인자에 넣어주면, 동적으로 변하는 그래픽을 그릴 수 있습니다.

#### 정리

```js
function drawCircle(ctx, coord, r, color) {
  ctx.fillStyle = color;
  ctx.beginPath();
  ctx.arc(coord.x, coord.y, r, 0, 2 * Math.PI);
  ctx.fill();
  ctx.stroke();
}

export default function Canvas({ radius }) {
  const canvasRef = useRef(null);

  function drawMain() {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");
    canvas.width = window.innerWidth * 0.7;
    canvas.height = window.innerHeight * 0.7;
    drawCircle(ctx, { x: 500, y: 150 }, radius, "skyblue");
    drawCircle(ctx, { x: 200, y: 350 }, radius, "lightyellow");
    drawCircle(ctx, { x: 500, y: 450 }, radius, "transparent");
  }

  useEffect(() => {
    drawMain();
  }, [radius]);

  return (
    <div className="canvas">
      <canvas ref={canvasRef}></canvas>
    </div>
  );
}
```

<br/>
<br/>
<br/>

[맨 위로](#)
