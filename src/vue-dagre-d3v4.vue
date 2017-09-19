<template>
    <svg id="svg-canvas"></svg>
</template>
<script>
    import * as d3 from 'd3';
    import dagreD3 from 'dagre-d3-webpack';

    export default {
        name: 'vue-dagre-d3v4',
        props: {
            nodes: {
                type: Array
            },
            links: {
                type: Array
            }
        },
        data () {
            return {
                rankdir: "LR",
                render: new dagreD3.render(),
                svg: d3.select("#svg-canvas"),
                svgGroup: this.svg.append("g")
            }
        },
        mounted: {
            createGraph(){

                let g = new dagre.graphlib.Graph().setGraph({rankdir: this.rankdir});
                this.nodes.forEach(function(n){
                    g.setNode(n["id"], {"label": n["label"], "count": n["count"], "class":"type-node"})
                });
                this.links.forEach(function(l){
                    g.setEdge(l["source"], l["target"], {"count": l["count"]})
                });

                this.render(d3.select("svg g"), g);

                let xCenterOffset = (this.svg.attr("width") - g.graph().width) / 2;
                this.svgGroup.attr("transform", "translate(" + xCenterOffset + ", 20)");
                this.svg.attr("height", g.graph().height + 40);
            }
        }
    }
</script>

<style>

</style>
