<template>
  <bar :xdata='xdata' :ydata='ydata' @changeYdata="changeYdata"/>
</template>

<script>
import bar from './components/bar';
export default {
    components:{
        bar
    },
    data(){
        return {
            xdata:['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
            ydata:[120, 200, 150, 80, 70, 110, 130],
        }
    },
    methods:{
        changeYdata(index,y){
            this.ydata[index] = y;
        }
    }
}
</script>

<style>

</style>