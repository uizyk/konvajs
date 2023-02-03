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
      stage: null,
      layer: null,
      rect1: null,
      rect2: null,
      circ1: null,
      circ2: null,
      // getLineGuideStops: this.getLineGuideStops(),
      // getObjectSnappingEdges: this.getObjectSnappingEdges(),
      // getGuides:  this.getGuides(),
      // drawGuides: this.drawGuides(),


      selectionRectPos: {
        x1: 0,
        y1: 0,
        x2: 0,
        y2: 0,
      },
      width: window.innerWidth,
      height: window.innerHeight
    };

  },
  methods: {
        // were can we snap our objects?
        getLineGuideStops(skipShape) {
        // we can snap to stage borders and the center of the stage
        const vertical = [0, this.width / 2, this.width];
        const horizontal = [0, this.height / 2, this.height];
        // const shapes = this.stage.find('.circle, .rect');
        // and we snap over edges and center of each object on the canvas
        this.stage.find('.object').forEach((guideItem) => {
          if (guideItem === skipShape) {
            return;
          }
          const box = guideItem.getClientRect();
          // and we can snap to all edges of shapes
          vertical.push([box.x, box.x + box.width, box.x + box.width / 2]);
          horizontal.push([box.y, box.y + box.height, box.y + box.height / 2]);
        });
        return {
          vertical: vertical.flat(),
          horizontal: horizontal.flat(),
        };
      },

      // what points of the object will trigger to snapping?
      // it can be just center of the object
      // but we will enable all edges and center
      getObjectSnappingEdges(node) {
        const box = node.getClientRect();
        const absPos = node.absolutePosition();

        return {
          vertical: [
            {
              guide: Math.round(box.x),
              offset: Math.round(absPos.x - box.x),
              snap: 'start',
            },
            {
              guide: Math.round(box.x + box.width / 2),
              offset: Math.round(absPos.x - box.x - box.width / 2),
              snap: 'center',
            },
            {
              guide: Math.round(box.x + box.width),
              offset: Math.round(absPos.x - box.x - box.width),
              snap: 'end',
            },
          ],
          horizontal: [
            {
              guide: Math.round(box.y),
              offset: Math.round(absPos.y - box.y),
              snap: 'start',
            },
            {
              guide: Math.round(box.y + box.height / 2),
              offset: Math.round(absPos.y - box.y - box.height / 2),
              snap: 'center',
            },
            {
              guide: Math.round(box.y + box.height),
              offset: Math.round(absPos.y - box.y - box.height),
              snap: 'end',
            },
          ],
        };
      },

      // find all snapping possibilities
      getGuides(lineGuideStops, itemBounds) {

        const resultV = [];
        const resultH = [];

        lineGuideStops.vertical.forEach((lineGuide) => {
          itemBounds.vertical.forEach((itemBound) => {
            const diff = Math.abs(lineGuide - itemBound.guide);
            // if the distance between guild line and object snap point is close we can consider this for snapping
            if (diff < 5) {
              resultV.push({
                lineGuide: lineGuide,
                diff: diff,
                snap: itemBound.snap,
                offset: itemBound.offset,
              });
            }
          });
        });

        lineGuideStops.horizontal.forEach((lineGuide) => {
          itemBounds.horizontal.forEach((itemBound) => {
            const diff = Math.abs(lineGuide - itemBound.guide);
            if (diff < 5) {
              resultH.push({
                lineGuide: lineGuide,
                diff: diff,
                snap: itemBound.snap,
                offset: itemBound.offset,
              });
            }
          });
        });

        const guides = [];

        // find closest snap
        const minV = resultV.sort((a, b) => a.diff - b.diff)[0];
        const minH = resultH.sort((a, b) => a.diff - b.diff)[0];
        if (minV) {
          guides.push({
            lineGuide: minV.lineGuide,
            offset: minV.offset,
            orientation: 'V',
            snap: minV.snap,
          });
        }
        if (minH) {
          guides.push({
            lineGuide: minH.lineGuide,
            offset: minH.offset,
            orientation: 'H',
            snap: minH.snap,
          });
        }

       return guides;

      },

      drawGuides(guides) {
        guides.forEach((lg) => {
          if (lg.orientation === 'H') {
            const line = new Konva.Line({
              points: [-6000, 0, 6000, 0],
              stroke: 'rgb(0, 161, 255)',
              strokeWidth: 1,
              name: 'guid-line',
              dash: [4, 6],
            });
            this.layer.add(line);
            line.absolutePosition({
              x: 0,
              y: lg.lineGuide,
            });
          } else if (lg.orientation === 'V') {
            const line = new Konva.Line({
              points: [0, -6000, 0, 6000],
              stroke: 'rgb(0, 161, 255)',
              strokeWidth: 1,
              name: 'guid-line',
              dash: [4, 6],
            });
            this.layer.add(line);
            line.absolutePosition({
              x: lg.lineGuide,
              y: 0,
            });
          }
        });
      }
    },
  mounted() {
    const stage = new Konva.Stage({
      container: this.$refs.container,
      width: this.width,
      height: this.height,
    });
    this.stage = stage;

    const layer = new Konva.Layer();
    this.layer = layer;
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
      name: "object",
      id: "rect1"
    });
    this.rect1 = rect1;
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
      name: "object",
      id: "rect2"
    });
    this.rect2 = rect2;
    layer.add(rect2);

    const circ1 = new Konva.Circle({
      x: 400,
      y: 70,
      radius: 50,
      draggable: true,
      fill: "purple",
      name: "object",
      stroke: "black",
      strokeWidth: 4,
    });
    this.circ1 = circ1;
    layer.add(circ1);

    const circ2 = new Konva.Circle({
      x: 400,
      y: 350,
      radius: 50,
      draggable: true,
      fill: "yellow",
      name: "object",
      stroke: "black",
      strokeWidth: 4,
    });
    this.circ2 = circ2;
    layer.add(circ2);

    //////////// GUIDELINES //////////////

    layer.on('dragmove', (e) => {

      // clear all previous lines on the screen
      layer.find('.guid-line').forEach((l) => l.destroy());

      // find possible snapping lines
      const lineGuideStops = this.getLineGuideStops(e.target);

      // find snapping edges of current object
      const itemBounds = this.getObjectSnappingEdges(e.target);
    
      // now find where we can snap current object
      const guides = this.getGuides(lineGuideStops, itemBounds);

      // do nothing if no snapping
      if (!guides.length) {
        return;
      }

      this.drawGuides(guides);

      const absPos = e.target.absolutePosition();

      // now force object position
      guides.forEach((lg) => {
        switch (lg.snap) {
          case 'start': {
            switch (lg.orientation) {
              case 'V': {
                absPos.x = lg.lineGuide + lg.offset;
                break;
              }
              case 'H': {
                absPos.y = lg.lineGuide + lg.offset;
                break;
              }
            }
            break;
          }
          case 'center': {
              switch (lg.orientation) {
                case 'V': {
                  absPos.x = lg.lineGuide + lg.offset;
                  break;
                }
                case 'H': {
                  absPos.y = lg.lineGuide + lg.offset;
                  break;
                }
              }
              break;
            }
          case 'end': {
            switch (lg.orientation) {
              case 'V': {
                absPos.x = lg.lineGuide + lg.offset;
                break;
              }
              case 'H': {
                absPos.y = lg.lineGuide + lg.offset;
                break;
              }
            }
            break;
          }
        }
      });
      e.target.absolutePosition(absPos);
    });

    layer.on('dragend', () => {
      // clear all previous lines on the screen
      this.layer.find('.guid-line').forEach((l) => l.destroy());
    })

    ////////////TRANSFORMER///////////////

    const tr = new Konva.Transformer({
      rotationSnaps: [0, 90, 180, 270]
    });
    layer.add(tr);


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

      const shapes = stage.find(".object");
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
