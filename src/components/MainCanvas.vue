<template>
  <div>
    <div ref="container"></div>
  </div>
</template>
<script>
/* eslint-disable */
import Konva from "konva";

export default {
  name: "MyComponent",
  data() {
    return {
      selectionRectPos: {
        x1: 0,
        y1: 0,
        x2: 0,
        y2: 0,
      },
    };
  },
  methods: {
    // LINE GUIDE SNAP
    // getLineGuideStops(skipShape) {
    //   const vertical = [0, stage.width() / 2, stage.width()];
    //   const horizontal = [0, stage.height() / 2, stage.height()];

    //   stage.find(".rect, .circle").forEach((guideItem) => {
    //     if (guideItem === skipShape) {
    //       return;
    //     }
    //     const box = guideItem.getClientRect();
    //     vertical.push([box.x, box.x + box.width, box.x + box.width / 2]);
    //     horizontal.push([box.y, box.y + box.height, box.y + box.height / 2]);
    //   });
    //   return {
    //     vertical: vertical.flat(),
    //     horizontal: horizontal.flat(),
    //   };
    // },

    // getShapeSnappingEdges(node) {
    //   const box = node.getClientRect();
    //   const absPos = node.absolutePosition();

    //   return {
    //     vertical: [
    //       {
    //         guide: Math.round(box.x),
    //         offset: Math.round(absPos.x - box.x),
    //         snap: "start",
    //       },
    //       {
    //         guide: Math.round(box.x + box.width / 2),
    //         offset: Math.round(absPos.x - box.x - box.width / 2),
    //         snap: "center",
    //       },
    //       {
    //         guide: Math.round(box.x + box.width),
    //         offset: Math.round(absPos.x - box.x - box.width),
    //         snap: "end",
    //       },
    //     ],
    //     horizontal: [
    //       {
    //         guide: Math.round(box.y),
    //         offset: Math.round(absPos.y - box.y),
    //         snap: "start",
    //       },
    //       {
    //         guide: Math.round(box.y + box.height / 2),
    //         offset: Math.round(absPos.y - box.y - box.height / 2),
    //         snap: "center",
    //       },
    //       {
    //         guide: Math.round(box.y + box.height),
    //         offset: Math.round(absPos.y - box.y - box.height),
    //         snap: "end",
    //       },
    //     ],
    //   };
    // },

    // getGuides(lineGuideStops, itemBounds) {
    //   const resultV = [];
    //   const resultH = [];

    //   lineGuideStops.vertical.forEach((lineGuide) => {
    //     itemBounds.vertical.forEach((itemBound) => {
    //       const diff = Math.abs(lineGuide - itemBound.guide);
    //       if (diff < GUIDELINE_OFFSET) {
    //         resultV.push({
    //           lineGuide: lineGuide,
    //           diff: diff,
    //           snap: itemBound.snap,
    //           offset: itemBound.offset,
    //         });
    //       }
    //     });
    //   });

    //   lineGuideStops.horizontal.forEach((lineGuide) => {
    //     itemBounds.horizontal.forEach((itemBound) => {
    //       const diff = Math.abs(lineGuide - itemBound.guide);
    //       if (diff < GUIDELINE_OFFSET) {
    //         resultH.push({
    //           lineGuide: lineGuide,
    //           diff: diff,
    //           snap: itemBound.snap,
    //           offset: itemBound.offset,
    //         });
    //       }
    //     });
    //   });

    //   let guides = [];

    //   // find closest snap
    //   var minV = resultV.sort((a, b) => a.diff - b.diff)[0];
    //   var minH = resultH.sort((a, b) => a.diff - b.diff)[0];
    //   if (minV) {
    //     guides.push({
    //       lineGuide: minV.lineGuide,
    //       offset: minV.offset,
    //       orientation: "V",
    //       snap: minV.snap,
    //     });
    //   }
    //   if (minH) {
    //     guides.push({
    //       lineGuide: minH.lineGuide,
    //       offset: minH.offset,
    //       orientation: "H",
    //       snap: minH.snap,
    //     });
    //   }
    //   return guides;
    // },
    // drawGuides(guides) {
    //   guides.forEach((lg) => {
    //     if (lg.orientation === "H") {
    //       var line = new Konva.Line({
    //         points: [-6000, 0, 6000, 0],
    //         stroke: "rgb(0, 161, 255)",
    //         strokeWidth: 1,
    //         name: "guid-line",
    //         dash: [4, 6],
    //       });
    //       layer.add(line);
    //       line.absolutePosition({
    //         x: 0,
    //         y: lg.lineGuide,
    //       });
    //     } else if (lg.orientation === "V") {
    //       var line = new Konva.Line({
    //         points: [0, -6000, 0, 6000],
    //         stroke: "rgb(0, 161, 255)",
    //         strokeWidth: 1,
    //         name: "guid-line",
    //         dash: [4, 6],
    //       });
    //       layer.add(line);
    //       line.absolutePosition({
    //         x: lg.lineGuide,
    //         y: 0,
    //       });
    //     }
    //   });
    // },
  },
  mounted() {
    const stage = new Konva.Stage({
      container: this.$refs.container,
      width: window.innerWidth,
      height: window.innerHeight,
    });

    const layer = new Konva.Layer();
    stage.add(layer);

    ///////////////SHAPES//////////////

    const rect1 = new Konva.Rect({
      x: 50,
      y: 50,
      width: 100,
      height: 100,
      fill: "red",
      stroke: "black",
      strokeWidth: 4,
      draggable: true,
      name: "rect",
    });
    layer.add(rect1);

    const rect2 = new Konva.Rect({
      x: 200,
      y: 50,
      width: 100,
      height: 100,
      fill: "dodgerblue",
      stroke: "black",
      strokeWidth: 4,
      draggable: true,
      name: "rect",
    });
    layer.add(rect2);

    const circ1 = new Konva.Circle({
      x: 400,
      y: 70,
      radius: 50,
      draggable: true,
      fill: "purple",
      name: "circle",
      stroke: "black",
      strokeWidth: 4,
    });
    layer.add(circ1);

    const circ2 = new Konva.Circle({
      x: 400,
      y: 350,
      radius: 50,
      draggable: true,
      fill: "yellow",
      name: "circle",
      stroke: "black",
      strokeWidth: 4,
    });
    layer.add(circ2);

    ////////////TRANSFORMER///////////////

    const tr = new Konva.Transformer();
    layer.add(tr);

    tr.nodes([rect1, rect2]);

    ////////////////SELECTION RECT//////////
    const selectionRect = new Konva.Rect({
      fill: "rgba(0,0,255,0.5)",
      visible: false,
    });
    layer.add(selectionRect);

    stage.on("mousedown touchstart", (e) => {
      if (e.target !== stage) {
        return;
      }
      e.evt.preventDefault();
      let pos = this.selectionRectPos;
      let stagePos = stage.getPointerPosition();
      pos.x1 = stagePos.x;
      pos.y1 = stagePos.y;
      pos.x2 = stagePos.x;
      pos.y2 = stagePos.y;

      selectionRect.visible(true);
      selectionRect.width(0);
      selectionRect.height(0);
    });

    stage.on("mousemove touchmove", (e) => {
      if (!selectionRect.visible()) {
        return;
      }
      e.evt.preventDefault();
      let pos = this.selectionRectPos;
      pos.x2 = stage.getPointerPosition().x;
      pos.y2 = stage.getPointerPosition().y;

      selectionRect.setAttrs({
        x: Math.min(pos.x1, pos.x2),
        y: Math.min(pos.y1, pos.y2),
        width: Math.abs(pos.x2 - pos.x1),
        height: Math.abs(pos.y2 - pos.y1),
      });
    });

    stage.on("mouseup touchend", (e) => {
      if (!selectionRect.visible()) {
        return;
      }
      e.evt.preventDefault();
      setTimeout(() => {
        selectionRect.visible(false);
      });

      const shapes = stage.find(".rect, .circle");
      const box = selectionRect.getClientRect();
      const selected = shapes.filter((shape) => {
        return Konva.Util.haveIntersection(box, shape.getClientRect());
      });

      tr.nodes(selected);
      console.log(selected);
    });

    stage.on("click tap", (e) => {
      // if (selectionRect.visible()) {
      //   return;
      // }

      if (e.target === stage) {
        tr.nodes([]);
        console.log("wat in the wuld");
        return;
      }

      // if (!e.target.hasName("rect")) {
      //   console.log("the weird one")
      //   return;
      // }

      // do we pressed shift or ctrl?
      const metaPressed = e.evt.shiftKey || e.evt.ctrlKey || e.evt.metaKey;
      const isSelected = tr.nodes().indexOf(e.target) >= 0;

      if (!metaPressed && !isSelected) {
        // if no key pressed and the node is not selected
        // select just one
        tr.nodes([e.target]);
      } else if (metaPressed && isSelected) {
        // if we pressed keys and node was selected
        // we need to remove it from selection:
        const nodes = tr.nodes().slice(); // use slice to have new copy of array
        // remove node from array
        nodes.splice(nodes.indexOf(e.target), 1);
        tr.nodes(nodes);
      } else if (metaPressed && !isSelected) {
        // add the node into selection
        const nodes = tr.nodes().concat([e.target]);
        tr.nodes(nodes);
      }
      console.log("wat");
    });
  },
};
</script>
