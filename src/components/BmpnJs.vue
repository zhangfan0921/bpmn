<template>
  <div id="app">
    <div class="containers">
      <!-- 创建一个canvas画布 npmn-js是通过canvas实现绘图的，并设置ref让vue获取到element -->
      <div class="canvas" ref="canvas"></div>
      <div class="panel" id="js-properties-panel"></div>
    </div>
  </div>
</template>

<script>
  import BpmnModeler from "bpmn-js/lib/Modeler";
  //import propertiesProviderModule from "bpmn-js-properties-panel/lib/provider/camunda";
  import propertiesPanelModule from "bpmn-js-properties-panel";
  import camundaModdleDescriptor from "camunda-bpmn-moddle/resources/camunda";
  import propertiesProviderModule from 'houtaroy-bpmn-js-properties-panel-activiti/lib/provider/activiti';
  import customTranslate from "../customTranslate/customTranslate";

  export default {
    name: "BmpnJs.vue",
    components: {},
    mounted() {
      this.bpmninit();
    },
    data() {
      return {
        bpmnModeler: null,
        canvas: null,
        //流程图初始模板代码
        bpmnTemplate: `<?xml version="1.0" encoding="UTF-8"?>
        <definitions xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:omgdc="http://www.omg.org/spec/DD/20100524/DC" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" id="sid-38422fae-e03e-43a3-bef4-bd33b32041b2" targetNamespace="http://bpmn.io/bpmn" exporter="bpmn-js (https://demo.bpmn.io)" exporterVersion="6.0.4">
          <process id="Process_1" isExecutable="true">
            <startEvent id="StartEvent_1acpsqt" />
          </process>
          <bpmndi:BPMNDiagram id="BpmnDiagram_1">
            <bpmndi:BPMNPlane id="BpmnPlane_1" bpmnElement="Process_1">
              <bpmndi:BPMNShape id="StartEvent_1acpsqt_di" bpmnElement="StartEvent_1acpsqt">
                <omgdc:Bounds x="152" y="82" width="36" height="36" />
              </bpmndi:BPMNShape>
            </bpmndi:BPMNPlane>
          </bpmndi:BPMNDiagram>
        </definitions>`
      };
    },
    methods: {
      bpmninit () {
        // 获取画布 element
        this.canvas = this.$refs.canvas;

        var customTranslateModule = {
          translate: [ 'value', customTranslate ]
        };
        // 创建Bpmn对象
        this.bpmnModeler = new BpmnModeler({
          // 设置bpmn的绘图容器为上门获取的画布 element
          container: this.canvas,

          // 加入工具栏支持
          propertiesPanel: {
            parent: "#js-properties-panel"
          },
          additionalModules: [propertiesProviderModule, propertiesPanelModule, customTranslateModule],
          moddleExtensions: {
            camunda: camundaModdleDescriptor
          }
        });

        this. initDiagram (this.bpmnTemplate);
      },
      initDiagram(bpmn) {
        // 将xml导入Bpmn-js建模器
        this.bpmnModeler.importXML(bpmn, err => {
          if (err) {
            this.$Message.error("打开模型出错,请确认该模型符合Bpmn2.0规范");
          }
        });
      },
    }
  }
</script>

<style lang="scss">
  @import 'bpmn-js/dist/assets/diagram-js.css';
  @import 'bpmn-js/dist/assets/bpmn-font/css/bpmn.css';
  @import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-codes.css';
  @import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-embedded.css';
  /*右边工具栏样式*/
  @import 'bpmn-js-properties-panel/dist/assets/bpmn-js-properties-panel';
  .containers{
    position: absolute;
    background-color: #ffffff;
    width: 100%;
    height: 100%;
    right: 0;
    top: 0;
  }
  .canvas{
    width: 100%;
    height: 100%;
    background: url("data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDAiIGhlaWdodD0iNDAiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGRlZnM+PHBhdHRlcm4gaWQ9ImEiIHdpZHRoPSI0MCIgaGVpZ2h0PSI0MCIgcGF0dGVyblVuaXRzPSJ1c2VyU3BhY2VPblVzZSI+PHBhdGggZD0iTTAgMTBoNDBNMTAgMHY0ME0wIDIwaDQwTTIwIDB2NDBNMCAzMGg0ME0zMCAwdjQwIiBmaWxsPSJub25lIiBzdHJva2U9IiNlMGUwZTAiIG9wYWNpdHk9Ii4yIi8+PHBhdGggZD0iTTQwIDBIMHY0MCIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjZTBlMGUwIi8+PC9wYXR0ZXJuPjwvZGVmcz48cmVjdCB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiBmaWxsPSJ1cmwoI2EpIi8+PC9zdmc+");
  }
  .panel{
    position: absolute;
    right: 0;
    top: 0;
    width: 400px;
    height: 100%;
  }
  .buttons{
    position: absolute;
    left: 20px;
    bottom: 20px;
    &>li{
      display:inline-block;margin: 5px;
      &>a{
        color: #999;
        background: #eee;
        cursor: not-allowed;
        padding: 8px;
        border: 1px solid #ccc;
        &.active{
          color: #333;
          background: #fff;
          cursor: pointer;
        }
      }
    }
  }
  /* 右下角logo */
  .bjs-powered-by {
    display: none;
  }
</style>