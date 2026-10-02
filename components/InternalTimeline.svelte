<script>
  export let data = undefined;
  export let height = 80;
  export let title = "";
  export let legend = false;
  export let link = undefined;
  export let min = undefined;
  export let max = undefined;
  export let yAxisLabels = true;
  import ECharts from "@evidence-dev/components/viz/ECharts.svelte";
  import { navigating, page } from '$app/stores'
</script>

{#if legend}
  <div class="flex justify-end">
    <div class="bg-[#bfdbf7] text-white font-semibold mr-2 px-2">Sleep {$page.path}</div>
    <div class="bg-[#488f96] text-white font-semibold mr-2 px-2"> Feed!</div>
    <div class="bg-[#ffc857] text-white font-semibold mr-2 px-2">Diaper</div>
    <div class="bg-[#923d59] text-white font-semibold mr-2 px-2">
      Tummy time
    </div>
  </div>
{/if}

<ECharts
  config={{
    title: {
      show: title !== "", // Only show the title if it's not an empty string
      text: title, // Text from the prop
      left: "left", // Align it to the left
      top: yAxisLabels ? "30%" : "middle",
      link: link,
    },
    xAxis: {
      type: "time",
      boundaryGap: false,
      min: min,
      max: max,
    },
    yAxis: {
      show: false, // This hides the Y-axis
      type: "category",
      data: ["Events"],
      axisLine: { onZero: false },
    },
    grid: {
      backgroundColor: "#F1F1F1", // Light grey background within the grid
      show: true, // Ensures the grid itself is visible
      containLabel: false, // If true, the labels will be contained within the grid
      borderWidth: 0, // Optionally remove border if not needed
      right: 0,
      bottom: yAxisLabels ? 20 : 0,
      top: 0,
      // if title add left padding
      left: title !== "" ? title.length * 10 : 0,
    },
    tooltip: {
      trigger: "item",
      formatter: function (params) {
        // Convert timestamps back to more readable time strings (HH:MM)
        var options = { hour: "numeric", minute: "2-digit", hour12: false };
        var startDate = new Date(params.value[0]).toLocaleTimeString([], options);
        var endDate = new Date(params.value[1]).toLocaleTimeString([], options);
        var duration = (params.value[1] - params.value[0]) / 1000 / 60; // Duration in minutes
        return `<b>${params.name}</b><br/>${startDate} - ${endDate}<br/>(${duration} mins)<br/>Notes: ${params.data.end_condition}`;
      },
    },
    series: [
      {
        name: "Event Timeline",
        type: "custom",
        renderItem: function (params, api) {
          // todo: remove logging
          console.log("YO!!!!!")
          var categoryIndex = api.value(2);
          console.log("YO!!!")
          console.log(api)
          var start = api.coord([api.value(0), categoryIndex]);
          var end = api.coord([api.value(1), categoryIndex]);
          // var height = api.size([0, 1])[1];
          // todo: don't hardcode these!
          // var categoryIndex = api.value(2);
          // var start = [0, 0];
          // var end = [10, 10];
          var height = 120;
          return {
            type: "rect",
            shape: {
              x: start[0],
              y: start[1] - height / 2,
              width: end[0] - start[0],
              height: height,
            },
            style: api.style(),
          };
        },
        data: [...data].map((item) => ({
          value: [
            // new Date(item.start).getTime(), // Start time in milliseconds
            // new Date(
            //   item.bedtime_end || new Date(item.bedtime_start).getTime() + 600000
            new Date(item.row_num), // Start time in milliseconds
            new Date(
              item.row_num + 1
            ).getTime(), // End time or start time + default duration
            0, // Category index
          ],
          itemStyle: {
            color:
            "#488f96" // Default color for the rectangle
              // item.Type === "Sleep"
              //   ? "#bfdbf7"
              //   : item.Type === "Feed"
              //     ? "#488f96"
              //     : item.Type === "Diaper"
              //       ? "#ffc857"
              //       : "#923d59",
          },
          name: item.Type,
          end_condition: item["total_sleep_duration"],
        })),
      },
    ],
  }}
  height="{height}px"
  renderer="svg"
/>
