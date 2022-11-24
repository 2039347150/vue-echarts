<template>
  <div class="home-wrapper">
    <div id="main" ref="chart" style="width: 100vw; height: 100vh"></div>
  </div>
</template>

<script>
import hangzhou from "../assets/hanghzou.json";
export default {
  data() {
    return {
      mapData:'',
      options: {
        tooltip: {
          show: true,
          trigger: "item",
        },
        geo3D: {
          map: "china", //注册地图的名字
          roam: true, //开启鼠标缩放和平移漫游。默认不开启
          tooltip: {
            show: true,
            trigger: "axis",
          },
          itemStyle: {
            //整体板块的样式
            // color: "rgba(1, 16, 31, 0)", // 背景
            color: "#4189f2",
            opacity: 1, //透明度
            borderWidth: 1, // 边框宽度
            borderColor: "#fff", // 边框颜色
          },
          label: {
            show: true,
            textStyle: {
              color: "#00ff7f", //地图初始化区域字体颜色
              fontSize: 8,
              opacity: 1,
              // backgroundColor: "rgba(0,23,11,1)",
            },

            formatter: function (params) {
              return params.name;
            },
          },
          //当鼠标放上去  地区区域是否显示名称
          emphasis: {
            disabled:true,
            label: {
              show: true,
              textStyle: {
                color: "#fff",
                fontSize: 3,
                // backgroundColor: "rgba(0,23,11,1)",
              },
              formatter: function(res) {
                console.log('params:', res)
                return res.name
              }
            },
          },
          shading: "lambert",
          //光照阴影
          light: {
            //光照阴影
            main: {
              color: "#fff", //光照颜色
              intensity: 1.2, //光照强度
              //shadowQuality: 'high', //阴影亮度
              shadow: true, //是否显示阴影
              alpha: 60,
              beta: 10,
            },
            ambient: {
              intensity: 0.6,
            },
          },
          //用于鼠标控制地图旋转等功能
          viewControl: {
            // 用于鼠标的旋转，缩放等视角控制。
            projection: "perspective", // 投影方式，默认为透视投影'perspective'，也支持设置为正交投影'orthographic'。
            autoRotate: false, // 是否开启视角绕物体的自动旋转查看。[ default: false ]
            autoRotateDirection: "cw", // 物体自传的方向。默认是 'cw' 也就是从上往下看是顺时针方向，也可以取 'ccw'，既从上往下看为逆时针方向。
            autoRotateSpeed: 10, // 物体自传的速度。单位为角度 / 秒，默认为10 ，也就是36秒转一圈。
            autoRotateAfterStill: 3, // 在鼠标静止操作后恢复自动旋转的时间间隔。在开启 autoRotate 后有效。[ default: 3 ]
            damping: 0, // 鼠标进行旋转，缩放等操作时的迟滞因子，在大于等于 1 的时候鼠标在停止操作后，视角仍会因为一定的惯性继续运动（旋转和缩放）。[ default: 0.8 ]
            rotateSensitivity: 10, // 旋转操作的灵敏度，值越大越灵敏。支持使用数组分别设置横向和纵向的旋转灵敏度。默认为1, 设置为0后无法旋转。   rotateSensitivity: [1, 0]——只能横向旋转； rotateSensitivity: [0, 1]——只能纵向旋转。
            zoomSensitivity: 10, // 缩放操作的灵敏度，值越大越灵敏。默认为1,设置为0后无法缩放。
            panSensitivity: 10, // 平移操作的灵敏度，值越大越灵敏。默认为1,设置为0后无法平移。支持使用数组分别设置横向和纵向的平移灵敏度
            panMouseButton: "left", // 平移操作使用的鼠标按键，支持：'left' 鼠标左键（默认）;'middle' 鼠标中键 ;'right' 鼠标右键(注意：如果设置为鼠标右键则会阻止默认的右键菜单。)
            rotateMouseButton: "left", // 旋转操作使用的鼠标按键，支持：'left' 鼠标左键;'middle' 鼠标中键（默认）;'right' 鼠标右键(注意：如果设置为鼠标右键则会阻止默认的右键菜单。)

            distance: 200, // [ default: 100 ] 默认视角距离主体的距离，对于 grid3D 和 geo3D 等其它组件来说是距离中心原点的距离,对于 globe 来说是距离地球表面的距离。在 projection 为'perspective'的时候有效。
            minDistance: 40, // [ default: 40 ] 视角通过鼠标控制能拉近到主体的最小距离。在 projection 为'perspective'的时候有效。
            maxDistance: 400, // [ default: 400 ] 视角通过鼠标控制能拉远到主体的最大距离。在 projection 为'perspective'的时候有效。

            alpha: 40, // 视角绕 x 轴，即上下旋转的角度。配合 beta 可以控制视角的方向。[ default: 40 ]
            beta: 15, // 视角绕 y 轴，即左右旋转的角度。[ default: 0 ]
            minAlpha: -720, // 上下旋转的最小 alpha 值。即视角能旋转到达最上面的角度。[ default: 5 ]
            maxAlpha: 720, // 上下旋转的最大 alpha 值。即视角能旋转到达最下面的角度。[ default: 90 ]
            minBeta: -720, // 左右旋转的最小 beta 值。即视角能旋转到达最左的角度。[ default: -80 ]
            maxBeta: 720, // 左右旋转的最大 beta 值。即视角能旋转到达最右的角度。[ default: 80 ]

            center: [0, 0, 0], // 视角中心点，旋转也会围绕这个中心点旋转，默认为[0,0,0]。左右 上下 前后

            animation: true, // 是否开启动画。[ default: true ]
            animationDurationUpdate: 1000, // 过渡动画的时长。[ default: 1000 ]
            animationEasingUpdate: "cubicInOut", // 过渡动画的缓动效果。[ default: cubicInOut ]
          },
          //修改选中区域的样式
          regions: [
            {
              name: "淳安县",
              height: 20, //修改选中区域的三维高度
              selected: true,
              select: {
                areaColor: "red",
                color: "red",
              },
            },
          ],
          silent: false,
          regionHeight: 10, //修改整个地图的三维高度
        },
        // series: [
        //   {
        //     type: "map3D", //加载series数据
        //     map: "china",
        //     itemStyle: {
        //       color: "rgba(1, 16, 31, .2)",
        //       opacity: 1,
        //       borderColor: "rgba(1, 16, 31, .6)",
        //       borderWidth: 1,
        //     },
        //     label: {
        //       show: false,
        //     },
        //     emphasis: {
        //       label: {
        //         show: true,
        //       },
        //       itemStyle: {
        //         color: "rgba(1, 16, 31, 1)",
        //         opacity: 1,
        //       },
        //     },
        //     zlevel: 1,
        //   },
        // ],
      },
    };
  },
  methods: {
    getMap() {
      var myChart = this.$echarts.init(document.getElementById("main"));
      this.$echarts.registerMap("china", hangzhou);
      myChart.setOption(this.options);
      myChart.getZr().on("click",  function (clickparams) {
        debugger;
      });
    },
  },
  mounted() {
    this.getMap();
  },
};
</script>

<style>
</style>