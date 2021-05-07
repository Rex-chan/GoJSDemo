<template>
  <div style="text-align: center">
    <diagram
      ref="diag"
      :model-data="diagramData"
      @model-changed="modelChanged"
      @changed-selection="changedSelection"
      @text-edited="textEdited"
      @modified="modified"
      style="width: 100%"
    ></diagram>
    <button @click="addNode">Add Child to Gamma</button>
    <button @click="modifyStuff">Modify view model data without undo</button>
    <br />Current Node:
    <input v-model.lazy="currentNodeText" :disabled="currentNode === null" />
    <br />The saved GoJS Model:
    <textarea
      style="width: 100%; height: 200px"
      v-model="savedModelText"
    ></textarea>
  </div>
</template>

<script>
import diagram from "../components/GoDiagramWorkflow";

export default {
  name: "",
  components: {
    diagram,
  },
  data() {
    return {
      diagramData2: {
        class: "go.GraphLinksModel",
        linkFromPortIdProperty: "fromPort",
        linkToPortIdProperty: "toPort",
        nodeDataArray: [],
        linkDataArray: [],
      },
      diagramData: {
        class: "go.GraphLinksModel",
        linkFromPortIdProperty: "fromPort",
        linkToPortIdProperty: "toPort",
        nodeDataArray: [
          {
            category: "Command",
            text: "Command1",
            key: -2,
            loc: "-202.90624999999994 -369.4999999999998",
          },
          {
            category: "Command",
            title: "tsfsfsfsfsfsf",
            text: "Command2",
            key: -3,
            loc: "-109.79687500000006 -248.24999999999994",
          },
        ],
        linkDataArray: [
          {
            from: -2,
            to: -3,
            fromPort: "B",
            toPort: "T",
            curviness: -20,
            points: [
              -202.90625,
              -353.06227569580085,
              -202.90625,
              -343.06227569580085,
              -202.90625,
              -308.875,
              -109.796875,
              -308.875,
              -109.796875,
              -274.6877243041992,
              -109.796875,
              -264.6877243041992,
            ],
          },
        ],
      },
      currentNode: null,
      savedModelText: "",
      counter: 1, // used by addNode
      counter2: 4, // used by modifyStuff
    };
  },
  mounted() {},
  computed: {
    currentNodeText: {
      get: function () {
        let node = this.currentNode;
        if (node instanceof window.go.Node) {
          return node.data.text;
        } else {
          return "";
        }
      },
      set: function (val) {
        let node = this.currentNode;
        if (node instanceof window.go.Node) {
          let model = this.model();
          model.startTransaction();
          model.setDataProperty(node.data, "text", val);
          model.commitTransaction("edited text");
        }
      },
    },
  },
  methods: {
    model: function () {
      return this.$refs.diag.model();
    },
    updateDiagramFromData: function () {
      this.$refs.diag.updateDiagramFromData();
    },
    modelChanged: function (e) {
      if (e.isTransactionFinished) {
        this.savedModelText = e.model.toJson();
      }
    },
    changedSelection: function (e) {
      let node = e.diagram.selection.first();
      if (node instanceof window.go.Node) {
        this.currentNode = node;
        this.currentNodeText = node.data.text;
      } else {
        this.currentNode = null;
        this.currentNodeText = "";
      }
    },
    textEdited: function (e) {
      let data = this.diagramData;
      let nodeDataArray = data.nodeDataArray;
      let len = nodeDataArray.length;
      for (let i = 0; i < len; i++) {
        nodeDataArray[i]["text"] = nodeDataArray[i]["text"].replace(/：/g, ":");
        console.log(nodeDataArray[i]["text"]);
      }
      this.updateDiagramFromData();
    },
    modified: function (e) {},
    addNode: function () {
      let model = this.model();
      model.startTransaction();
      model.setDataProperty(model.findNodeDataForKey(4), "color", "purple");
      let data = { text: "NEW " + this.counter++, color: "yellow" };
      model.addNodeData(data);
      model.addLinkData({ from: 3, to: model.getKeyForNodeData(data) });
      model.commitTransaction("added Node and Link");
      let diagram = this.$refs.diag.diagram;
      diagram.select(diagram.findNodeForData(data));
    },
    modifyStuff: function () {
      let data = this.diagramData;
      data.nodeDataArray[0].color = "red";
      data.nodeDataArray.push({
        key: ++this.counter2,
        text: this.counter2.toString(),
        color: "orange",
      });
      data.linkDataArray.push({ from: 2, to: this.counter2 });
      this.updateDiagramFromData();
    },
  },
};
</script>

<style>
</style>
