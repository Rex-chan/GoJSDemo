<template>
  <div style="width: 100%; white-space: nowrap">
    <span
      style="
        border: 1px solid gray;
        display: inline-block;
        vertical-align: top;
        width: 120px;
        height: 400px;
      "
    >
      <div ref="myPaletteDiv" style="height: 400px">1111</div>
    </span>
    <span
      style="
        border: 1px solid gray;
        display: inline-block;
        vertical-align: top;
        width: 70%;
        height: 400px;
      "
    >
      <div ref="myDiagramDiv" style="height: 400px"></div>
    </span>
  </div>
</template>

<script>
let go = window.go;
let $ = go.GraphObject.make;

export default {
  name: "",
  props: ["modelData"],
  data() {
    return {
      diagram: null,
    };
  },
  // mounted() {
  //     let self = this;
  //     let myDiagram = $(go.Diagram, this.$refs.myDiagramDiv, {
  //       initialContentAlignment: go.Spot.Center,
  //       "undoManager.isEnabled": true,
  //       ModelChanged: function (e) {
  //         self.$emit("model-changed", e);
  //       },
  //       ChangedSelection: function (e) {
  //         self.$emit("changed-selection", e);
  //       },
  //       Modified: function (e) {
  //         self.$emit("modified", e);
  //       },
  //       TextEdited: function (e) {
  //         self.$emit("text-edited", e);
  //       },
  //       allowDrop: true,
  //     });
  //     myDiagram.nodeTemplateMap.add(
  //       "",
  //       $(
  //         go.Node,
  //         "Spot",
  //         this.nodeStyle(),
  //         $(
  //           go.Panel,
  //           "Auto",
  //           $(
  //             go.Shape,
  //             "Rectangle",
  //             { fill: "#00A9C9", stroke: null },
  //             new go.Binding("figure", "figure")
  //           ),
  //           $(
  //             go.TextBlock,
  //             "command",
  //             {
  //               font: "bold 11pt Helvetica, Arial, sans-serif",
  //               stroke: "whitesmoke",
  //               margin: 8,
  //               maxSize: new go.Size(160, NaN),
  //               wrap: go.TextBlock.WrapFit,
  //               editable: true,
  //             },
  //             new go.Binding("text").makeTwoWay()
  //           )
  //         ),
  //         // four named ports, one on each side:
  //         this.makePort("T", go.Spot.Top, false, true),
  //         this.makePort("L", go.Spot.Left, true, true),
  //         this.makePort("R", go.Spot.Right, true, true),
  //         this.makePort("B", go.Spot.Bottom, true, false)
  //       )
  //     );
  //     myDiagram.linkTemplate = $(
  //       go.Link,
  //       $(
  //         go.TextBlock, // this is a Link label
  //         new go.Binding("text", "text")
  //       ),
  //       // the whole link panel
  //       {
  //         routing: go.Link.AvoidsNodes,
  //         curve: go.Link.JumpOver,
  //         corner: 5,
  //         toShortLength: 4,
  //         relinkableFrom: true,
  //         relinkableTo: true,
  //         reshapable: true,
  //         resegmentable: true,
  //         // mouse-overs subtly highlight links:
  //         mouseEnter: function (e, link) {
  //           link.findObject("HIGHLIGHT").stroke = "rgba(30,144,255,0.2)";
  //         },
  //         mouseLeave: function (e, link) {
  //           link.findObject("HIGHLIGHT").stroke = "transparent";
  //         },
  //       },
  //       new go.Binding("points").makeTwoWay(),
  //       $(
  //         go.Shape, // the highlight shape, normally transparent
  //         {
  //           isPanelMain: true,
  //           strokeWidth: 8,
  //           stroke: "transparent",
  //           name: "HIGHLIGHT",
  //         }
  //       ),
  //       $(
  //         go.Shape, // the link path shape
  //         { isPanelMain: true, stroke: "gray", strokeWidth: 2 }
  //       ),
  //       $(
  //         go.Shape, // the arrowhead
  //         { toArrow: "standard", stroke: null, fill: "gray" }
  //       ),
  //       $(
  //         go.Panel,
  //         "Auto", // the link label, normally not visible
  //         { visible: true, name: "LABEL", segmentIndex: 2, segmentFraction: 0.5 },
  //         new go.Binding("visible", "visible").makeTwoWay(),
  //         $(
  //           go.Shape, // the label shape
  //           {
  //             fill: $(go.Brush, "Radial", {
  //               0: "rgb(240, 240, 240)",
  //               0.3: "rgb(240, 240, 240)",
  //               1: "rgba(240, 240, 240, 0)",
  //             }),
  //             stroke: null,
  //           }
  //         ),
  //         $(
  //           go.TextBlock,
  //           "Yes", // the label
  //           {
  //             textAlign: "center",
  //             font: "10pt helvetica, arial, sans-serif",
  //             stroke: "#333333",
  //             editable: false,
  //           },
  //           new go.Binding("text").makeTwoWay()
  //         )
  //       )
  //     );
  //     let myPalette = $(
  //       go.Palette,
  //       this.$refs.myPaletteDiv, // must name or refer to the DIV HTML element
  //       {
  //         "animationManager.duration": 800, // slightly longer than default (600ms) animation
  //         nodeTemplateMap: myDiagram.nodeTemplateMap, // share the templates used by myDiagram
  //         // nodeTemplate: myDiagram.nodeTemplate, // share the templates used by myDiagram
  //         model: new go.GraphLinksModel([
  //           // specify the contents of the Palette
  //           { category: "Start", text: "Start" },
  //           { category: "Command", text: "Command" },
  //           { category: "End", text: "End" },
  //         ]),
  //       }
  //     );
  //     console.log(myPalette);
  //     this.diagram = myDiagram;
  //     this.updateModel(this.modelData);
  //   },
  mounted() {
    var mySelf = this;
    const MAKE = go.GraphObject.make;
    mySelf.myDiagram = MAKE(go.Diagram, this.$refs.myDiagramDiv, {
      initialContentAlignment: go.Spot.Center, // 居中显示
      "draggingTool.isGridSnapEnabled": true,
      "resizingTool.isGridSnapEnabled": true,
      "undoManager.isEnabled": true, // 支持 Ctrl-Z 和 Ctrl-Y 操作
      "toolManager.hoverDelay": 100, //tooltip提示显示延时
      "toolManager.toolTipDuration": 10000, //tooltip持续显示时间
      //isReadOnly:true,//只读
      "grid.visible": true, //显示网格
      allowMove: true, //允许拖动
      // allowDragOut:true,
      allowDelete: false,
      allowCopy: false,
      allowClipboard: false,
      "toolManager.mouseWheelBehavior": go.ToolManager.WheelZoom, //有鼠标滚轮事件放大和缩小，而不是向上和向下滚动
      layout: MAKE(go.TreeLayout, {
        angle: 0,
        layerSpacing: 35,
      }),
    }); //构建gojs对象
    console.log(mySelf.myDiagram);
    mySelf.myDiagram.addDiagramListener("ObjectSingleClicked", function (e) {
      console.log(e.subject.part);
    });

    mySelf.myDiagram.addDiagramListener(
      "BackgroundSingleClicked",
      function (e) {
        console.log("Double-clicked at" + e.diagram.lastInput.documentPoint);
      }
    );

    mySelf.myDiagram.addDiagramListener("ClipboardPasted", function (e) {
      console.log("Pasted" + e.diagram.selection.count + "parts");
    });

    // define a simple Node template
    // 定义个简单的 Node 模板
    mySelf.myDiagram.nodeTemplate = MAKE(
      go.Node,
      new go.Binding("location", "loc", go.Point.parse),
      MAKE(go.Shape, "RoundedRectangle", {
        fill: "#44CCFF",
        stroke: "green",
        strokeWidth: 2,
        angle: 15,
      }),
      "Auto", //Vertical,Auto,Horizontal
      // 为整个Node背景设置为浅蓝色
      // { background: "#44CCFF" },

      // MAKE(go.Picture,
      //   // Pictures 应该指定宽高.
      //   // 当没有图片时显示红色的背景
      //   // 或者当图片为透明的时候也是.
      //   { source:"../assets/img/01.png",margin: 10, width: 50, height: 50, background: "red" },
      //   // Picture.source参数值与模型数据中的"source"字段绑定
      //   new go.Binding("source")),
      // MAKE(go.TextBlock,
      //   "Default Text",  // 初始化默认文本
      //   // 文字周围的空隙, 大号字体, 白色笔画:
      //   { margin: 12, stroke: "white", font: "bold 16px sans-serif",
      //     width:75,
      //     wrap: go.TextBlock.WrapDesiredSize
      //   },
      //   // TextBlock.text参数值与模型数据中的"name"字段绑定
      //   new go.Binding("text", "name1")),
      MAKE(
        go.Panel,
        "Horizontal",
        { padding: 5 },
        MAKE(
          go.Panel,
          "Vertical",
          MAKE(
            go.Picture,
            { margin: 10, width: 50, height: 50 },
            new go.Binding("source", "img")
          )
        ),
        MAKE(
          go.Panel,
          "Vertical",
          MAKE(
            go.TextBlock,
            "Default Text",
            { margin: 12, stroke: "white", font: "bold 16px sans-serif" },
            new go.Binding("text", "name1")
          )
          //   MAKE(
          //     go.TextBlock,
          //     { stroke: "red" },
          //     { margin: 5 },
          //     new go.Binding("text", "name2")
          //   ),
          //   MAKE(
          //     go.TextBlock,
          //     { background: "lightblue" },
          //     { margin: 5 },
          //     new go.Binding("text", "name3")
          //   )
        )
      ),
      {
        mouseEnter: function (e, node, prev) {
          console.log("mouseEnter");
        },
        mouseLeave: function (e, node, prev) {
          mySelf.detailShow = false;
        },
      },
      {
        toolTip: MAKE(
          go.Adornment,
          "Spot",
          //{background:"transparent" },
          MAKE(go.Shape, "RoundedRectangle", {
            // fill: "blue" ,
            height: 30,
            fill: MAKE(go.Brush, "Linear", {
              0.0: "blue",
              1.0: "red",
              start: go.Spot.Bottom,
              end: go.Spot.Top,
            }),
          }),
          MAKE(
            go.TextBlock,
            //{alignment:go.Spot.Top,alignmentFocus:go.Spot.Bottom,stroke:"red" },
            { margin: 4, stroke: "white" },
            new go.Binding("text", "name1")
          )
        ), // end of Adornment
      }
    );
    mySelf.myDiagram.linkTemplate = MAKE(
      go.Link,
      //{ curve: go.Link.Bezier },  // 贝塞尔曲线
      { routing: go.Link.Orthogonal, corner: 5 },
      MAKE(go.Shape, { strokeWidth: 2, stroke: "#e4393c" }),
      MAKE(go.Shape, { toArrow: "Standard", fill: "#000", stroke: null }), //箭头
      MAKE(
        go.TextBlock,
        {
          //margin: 20,
          stroke: "blue",
          //font: "14px sans-serif",
          //width:50,
          //wrap: go.TextBlock.WrapDesiredSize
        },
        new go.Binding("text", "linktext")
      ),
      {
        toolTip: MAKE(
          go.Adornment,
          "Auto",
          MAKE(go.Shape, { fill: "#FFFFCC" }),
          MAKE(go.TextBlock, { margin: 4 }, new go.Binding("text", "name1"))
        ), // end of Adornment
      }
    ); // the link shape
    // let myModel = MAKE(go.Model);//如果不需要连线可以用这样的方法创建model
    // let myModel = MAKE(go.GraphLinksModel);//也可以创建link model;需要配置myModel.linkDataArray 如下
    let myModel = MAKE(go.TreeModel);
    myModel.nodeDataArray = [
      // note that each node data object holds whatever properties it needs;
      // for this app we add the "name" and "source" properties
      {
        key: "1",
        name1: "设备1",
        name2: "设备11",
        name3: "设备12",
        img: require("../assets/logo.png"),
      },
      {
        key: "2",
        parent: "1",
        name1: "设备2",
        name2: "设备21",
        name3: "设备22",
        linktext: "link",
        img: require("../assets/90照明控制箱.svg"),
      },
      {
        key: "3",
        parent: "1",
        name1: "设备3",
        name2: "设备31",
        name3: "设备32",
        linktext: "link",
        img: require("../assets/gifdemo.gif"),
      },
      {
        key: "4",
        parent: "3",
        name1: "设备4",
        name2: "设备41",
        name3: "设备42",
        linktext: "link",
        img: require("../assets/logo.png"),
      },
      {
        key: "5",
        parent: "4",
        name1: "设备5",
        name2: "设备51",
        name3: "设备52",
        linktext: "link",
        img: require("../assets/logo.png"),
      },
    ];
    // myModel.linkDataArray = [
    //   {from:"1",to:"2"},
    //   {from:"1",to:"3"},
    //   {from:"1",to:"4"},
    //   {from:"1",to:"5"},
    // ];
    // function diagramInfo(myModel) {
    //   return "myModel:\n" + myModel.nodeDataArray.length + " nodes, " +myModel.linkDataArray.length + " links";
    // }
    // mySelf.myDiagram.toolTip = MAKE(go.Adornment, "Auto",
    //   MAKE(go.Shape, { fill: "#CCFFCC" }),
    //   MAKE(go.TextBlock, { margin: 4 },
    //     // use a converter to display information about the diagram model
    //     new go.Binding("text", "", diagramInfo))
    // );
    mySelf.myDiagram.model = myModel;
  },
  computed: {},
  methods: {
    makePort(name, spot, output, input) {
      return $(go.Shape, "Circle", {
        fill: "transparent",
        stroke: null, // this is changed to 'white' in the showPorts function
        desiredSize: new go.Size(8, 8),
        alignment: spot,
        alignmentFocus: spot, // align the port on the main Shape
        portId: name, // declare this object to be a 'port'
        fromSpot: spot,
        toSpot: spot, // declare where links may connect at this port
        fromLinkable: output,
        toLinkable: input, // declare whether the user may draw links to/from here
        cursor: "pointer", // show a different cursor to indicate potential link point
      });
    },
    nodeStyle() {
      return [
        new go.Binding("location", "loc", go.Point.parse).makeTwoWay(
          go.Point.stringify
        ),
        {
          locationSpot: go.Spot.Center,
          mouseEnter: (e, obj) => {
            this.showPorts(obj.part, true);
          },
          mouseLeave: (e, obj) => {
            this.showPorts(obj.part, false);
          },
        },
      ];
    },
    showPorts(node, show) {
      let diagram = node.diagram;
      if (!diagram || diagram.isReadOnly || !diagram.allowLink) return;
      node.ports.each(function (port) {
        port.stroke = show ? "white" : null;
      });
    },
    model: function () {
      return this.diagram.model;
    },
    updateModel: function (val) {
      // No GoJS transaction permitted when replacing Diagram.model.
      if (val instanceof go.Model) {
        this.diagram.model = val;
      } else {
        let m = new go.GraphLinksModel();
        if (val) {
          for (let p in val) {
            m[p] = val[p];
          }
        }
        this.diagram.model = m;
      }
    },
    updateDiagramFromData: function () {
      this.diagram.startTransaction();
      // This is very general but very inefficient.
      // It would be better to modify the diagramData data by calling
      // Model.setDataProperty or Model.addNodeData, et al.
      this.diagram.updateAllRelationshipsFromData();
      this.diagram.updateAllTargetBindings();
      this.diagram.commitTransaction("updated");
    },
  },
};
</script>

<style>
</style>
