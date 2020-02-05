html:
```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>svg demo</title>
  <link rel="stylesheet" href="./style.css">

</head>

<body>
  <div></div>
  <!-- partial:index.partial.html -->
  <svg id="svg1" width="1960" height="1500"></svg>
  <!-- partial -->
  <script src='https://cdnjs.cloudflare.com/ajax/libs/d3/5.12.0/d3.min.js'></script>
  <script src="./script.js"></script>

</body>

</html>
```

---

js:
```

function createNode(options) {
  var width = 160;
  var height = 30;

  //多个输出时需要重新计算高度
  if (options.output > 2) {
    height += (options.output - 2) * 15;
  }

  var svg = d3.select("svg")
    .on('mousemove', function () {
      var coordinates = d3.mouse(this);
      var x = coordinates[0];
      var y = coordinates[1];
      var x = d3.event.pageX - document.getElementById("svg1").getBoundingClientRect().x
      var y = d3.event.pageY - document.getElementById("svg1").getBoundingClientRect().y
      d3.select('div').text(x + "," + y);
    });

  var g_node = svg
    .append("g")
    .attr('name', '节点')
    .attr("transform", "translate(" + options.x + "," + options.y + ")");

  width = drawText(height, options.text, options.logo_position, g_node, true);


  /*主体矩形*/
  g_node
    .append("rect")
    .attr('name', '边框矩形')
    .attr("rx", 5)
    .attr("ry", 5)
    .attr("fill", "#c6dbef")
    .attr("width", width)
    .attr("height", height)
    .attr('class', 'node');


  /*图标的小矩形组*/
  var logo_x = options.logo_position === 'left' ? 0 : width - 30;
  var g_logo = g_node
    .append("g")
    .attr('name', 'LOGO的矩形父节点')
    .attr("x", 0)
    .attr("y", 0)
    .attr("transform", "translate(" + logo_x + ",0)");
  g_logo
    .append("rect")
    .attr('name', 'LOGO矩形')
    .attr("x", 0)
    .attr("y", 0)
    .attr("width", 30)
    .attr("stroke", "none")
    .attr("fill", "#000")
    .attr("fill-opacity", 0.05)
    .attr("height", height)
    .attr('class', 'node_icon_shade');

  g_logo
    .append("image")
    .attr('name', 'LOGO图片')
    .attr(
      "href",
      "https://raw.githubusercontent.com/willxiang/code-note/master/img/split.png"
    )
    .attr("x", 5)
    .attr("y", 0)
    .attr("width", 20)
    .attr("height", height)
    .attr('class', 'node_icon');

  g_logo.append('path')
    .attr('name', 'LOGO竖线')
    .attr('stroke-opacity', 0.1)
    .attr('stroke', '#000')
    .attr('stroke-width', 1)
    .attr('class', 'node_icon_shade_border');

  /*文本*/
  drawText(height, options.text, options.logo_position, g_node, false);

  /*错误提示*/
  if (options.error) {
    g_node
      .append('image')
      .attr('name', '错误图片')
      .attr('class', 'node_error')
      .attr('x', width - 20)
      .attr('y', height - 5)
      .attr('width', 10)
      .attr('height', 10)
      .attr('href', 'https://raw.githubusercontent.com/willxiang/code-note/master/img/node-error.png');

  }

  /*算子包*/
  if (options.suanzi) {
    g_node
      .append('image')
      .attr('name', '算子包图片')
      .attr('class', 'node_error')
      .attr('x', width - 30)
      .attr('y', -5)
      .attr('width', 10)
      .attr('height', 10)
      .attr('href', 'https://i.loli.net/2019/12/11/B6eugmyb47PLJjE.png');

  }




  var y_port = height / 2;
  /*输入点*/
  if (options.input > 0) {
    var g_input = g_node
      .append("g")
      .attr('name', 'input组')
      .attr("transform", "translate(-5," + (y_port - 5) + ")");

    g_input
      .append("rect")
      .attr('name', 'input矩形')
      .attr("rx", "3")
      .attr("ry", "3")
      .attr("width", "10")
      .attr("height", "10")
      .attr("class", "port");
  }

  /*输出点*/
  if (options.output > 0) {
    //只有一个输出则按默认情况处理
    if (options.output === 1) {
      var g_output = g_node
        .append("g")
        .attr('name', 'output组')
        .attr("transform", "translate(" + (width - 5) + "," + (y_port - 5) + ")");
      g_output
        .append("rect")
        .attr('name', 'output矩形')
        .attr("rx", "3")
        .attr("ry", "3")
        .attr("width", "10")
        .attr("height", "10")
        .attr("class", "port");
      // g_output
      // .append('text')
      // .attr('y',9)
      // .attr('x',2)
      // .style('font-size','10px')
      // .text('1');
    }
    else {
      //每增加一个输出，总高度增加15，输出与输出之间空隙是13，输出大于1时，第一个输出的 y = output + 1.5
      var output_port_y = options.output + 1.5;
      for (var i = 0; i < options.output; i++) {
        var g_output = g_node
          .append("g")
          .attr('name', 'output组')
          .attr("transform", "translate(" + (width - 5) + "," + (13 * i + output_port_y) + ")");

        g_output
          .append("rect")
          .attr('name', 'output矩形' + (i + 1))
          .attr("rx", "3")
          .attr("ry", "3")
          .attr("width", "10")
          .attr("height", "10")
          .attr("class", "port");

        g_output
          .append('text')
          .attr('name', '数字' + (i + 1))
          .attr('y', 8)
          .attr('x', i > 8 ? 0 : 3)//数字位数大于2时，需要调整，暂不考虑三位数长度情况
          .style('font-size', '8px')
          .text((i + 1));
      }
    }

  }





  /*事件图标 */
  if (options.event) {
    var g_event = g_node
      .append('g')
      .attr('name', 'event节点组')
      .attr("transform", "translate(" + (width / 2 - 5) + "," + (height - 5) + ")");

    g_event
      .append("rect")
      .attr('name', 'event的矩形')
      .attr("rx", "3")
      .attr("ry", "3")
      .attr("width", "10")
      .attr("height", "10")
      .attr("class", "port");

  }


  var origX = 345;
  var origY = 35;

  var destX = 247;
  var destY = 99;
  var sc = 1;
  var d = generateLinkPath(width, height, origX, origY, destX, destY, sc);

  svg.append('path').attr('d', d)
    .attr('stroke', '#888')
    .attr('stroke-width', 3)
    .attr('fill', 'none')
}


/*获取当前文本的width，以便自适应调整 */
function drawText(height, text, logo_position, g_node, remove) {
  //先把这个文本画出来，然后通过getBoundingClientRect得到大小属性，如果remove为true则最后再删掉这个text。
  var padding = logo_position === 'left' ? 38 : 10;
  var randomId = "_" + Math.round(Math.random() * 100000000);
  g_node
    .append("text")
    .attr('name', '文字节点')
    .attr('id', randomId)
    .attr("x", padding)
    .attr("dy", ".35em")
    .attr("y", height / 2)
    .text(text);
  var width = document.getElementById(randomId).getBoundingClientRect().width + 60;
  if (remove) {
    g_node.selectAll('text').remove();
  }
  return width;
}

//MARK:配置
var options = {
  x: 250,
  y: 20,
  text: 'DOU',
  input: 1,//输入个数，暂只支持1个
  output: 1,//输出个数
  error: true,
  // event: true,
  suanzi: true,
  logo_position: 'left' // left or right
};


createNode(options);





function generateLinkPath(width, height, origX, origY, destX, destY, sc) {//MARK:generateLinkPath
  var node_width = width;
  var node_height = height;

  var dy = destY - origY;
  var dx = destX - origX;
  var delta = Math.sqrt(dy * dy + dx * dx);
  // var scale = lineCurveScale;//0.75
  var scale = 0.75;//0.75
  var scaleY = 0;
  if (dx * sc > 0) {
    if (delta < node_width) {
      scale = 0.75 - 0.75 * ((node_width - delta) / node_width);
      // scale += 2*(Math.min(5*node_width,Math.abs(dx))/(5*node_width));
      // if (Math.abs(dy) < 3*node_height) {
      //     scaleY = ((dy>0)?0.5:-0.5)*(((3*node_height)-Math.abs(dy))/(3*node_height))*(Math.min(node_width,Math.abs(dx))/(node_width)) ;
      // }
    }
  } else {
    scale = 0.4 - 0.2 * (Math.max(0, (node_width - Math.min(Math.abs(dx), Math.abs(dy))) / node_width));
  }
  if (dx * sc > 0) {
    return "M " + origX + " " + origY +
      " C " + (origX + sc * (node_width * scale)) + " " + (origY + scaleY * node_height) + " " +
      (destX - sc * (scale) * node_width) + " " + (destY - scaleY * node_height) + " " +
      destX + " " + destY
  } else {

    var midX = Math.floor(destX - dx / 2);
    var midY = Math.floor(destY - dy / 2);
    //
    if (dy === 0) {
      midY = destY + node_height;
    }
    var cp_height = node_height / 2;
    var y1 = (destY + midY) / 2
    var topX = origX + sc * node_width * scale;
    var topY = dy > 0 ? Math.min(y1 - dy / 2, origY + cp_height) : Math.max(y1 - dy / 2, origY - cp_height);
    var bottomX = destX - sc * node_width * scale;
    var bottomY = dy > 0 ? Math.max(y1, destY - cp_height) : Math.min(y1, destY + cp_height);
    var x1 = (origX + topX) / 2;
    var scy = dy > 0 ? 1 : -1;
    var cp = [
      // Orig -> Top
      [x1, origY],
      [topX, dy > 0 ? Math.max(origY, topY - cp_height) : Math.min(origY, topY + cp_height)],
      // Top -> Mid
      // [Mirror previous cp]
      [x1, dy > 0 ? Math.min(midY, topY + cp_height) : Math.max(midY, topY - cp_height)],
      // Mid -> Bottom
      // [Mirror previous cp]
      [bottomX, dy > 0 ? Math.max(midY, bottomY - cp_height) : Math.min(midY, bottomY + cp_height)],
      // Bottom -> Dest
      // [Mirror previous cp]
      [(destX + bottomX) / 2, destY]
    ];
    if (cp[2][1] === topY + scy * cp_height) {
      if (Math.abs(dy) < cp_height * 10) {
        cp[1][1] = topY - scy * cp_height / 2;
        cp[3][1] = bottomY - scy * cp_height / 2;
      }
      cp[2][0] = topX;
    }
    return "M " + origX + " " + origY +
      " C " +
      cp[0][0] + " " + cp[0][1] + " " +
      cp[1][0] + " " + cp[1][1] + " " +
      topX + " " + topY +
      " S " +
      cp[2][0] + " " + cp[2][1] + " " +
      midX + " " + midY +
      " S " +
      cp[3][0] + " " + cp[3][1] + " " +
      bottomX + " " + bottomY +
      " S " +
      cp[4][0] + " " + cp[4][1] + " " +
      destX + " " + destY
  }
}
```

---
`codepen`地址：
https://codepen.io/willxiang/pen/JjoGRdW