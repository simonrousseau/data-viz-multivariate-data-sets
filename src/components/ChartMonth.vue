<script>
import { Line } from 'vue-chartjs'
import zoom from 'chartjs-plugin-zoom';

export default {
  extends: Line,
  props: ['chartdata', 'options'],
  mounted () {
    this.addPlugin(zoom);
    this.renderChart(this.chartdata, this.options)
  }
}
</script>

<style>
</style>