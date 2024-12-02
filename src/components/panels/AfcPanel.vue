<template>
  <div class="afc-panel-wrapper">
      <panel :icon="mdiAdjust"
             :title="'AFC Spools'"
             card-class="afc-panel"
             :collapsible="true"
             :expanded="true">
          <template #buttons>
              <v-btn icon tile :title="'Refresh AFC Spools'" @click="fetchSpoolData">
                  <v-icon>{{ mdiRefresh }}</v-icon>
              </v-btn>
              <!--<v-menu :offset-y="true" :close-on-content-click="true" left>
          <template v-slot:activator="{ on, attrs }">
            <v-btn icon tile v-bind="attrs" v-on="on">
              <v-icon>{{ mdiDotsVertical }}</v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item>
              <v-list-item-title>Filament Icon Style</v-list-item-title>
            </v-list-item>
            <v-list-item>
              <v-checkbox
                v-model="mainsailIconSwitch"
                label="Mainsail Theme"
                @change="onIconStyleChange('mainsail')"
              ></v-checkbox>
            </v-list-item>-->
              <!--<v-list-item>
              <v-checkbox
                v-model="klipperScreenIconSwitch"
                label="KlipperScreen Theme"
                @change="onIconStyleChange('klipperscreen')"
              ></v-checkbox>
            </v-list-item>
            <v-list-item>
              <v-checkbox
                v-model="spoolManIconSwitch"
                label="Spoolman Theme"
                @change="onIconStyleChange('spoolman')"
              ></v-checkbox>
            </v-list-item>-->
              <!--<v-list-item>
              <v-checkbox
                v-model="noIconSwitch"
                label="No Icon"
                @change="onIconStyleChange('none')"
              ></v-checkbox>
            </v-list-item>
          </v-list>
        </v-menu>-->
          </template>
          <div class="printer-section">
              <h2 class="unit-title">Printer</h2>
                  <div style="margin-right: 10px;" >
                      <span class="tool-status">
                          <strong>Tool Status:</strong>
                          <span :class="{
                                'status-light': true,
                                'status-green': toolStartSensorStatus,
                                'status-red': !toolStartSensorStatus,
                                }">
                          </span>
                      </span>
                  </div>
                  <div>
                      <strong>Buffer Status:</strong> {{ BufferStatus }}
                  </div>
          </div>
          <div v-for="(unit, unitName) in unitsData"
               :key="unitName"
               class="unit-section">
              <div class="unit-title">
                  <BoxTurtle-icon v-if="getType(unitName) ==='Box_Turtle'" style="width: 12%; float: left" class="mr-3" />
                  <h2 class="unit-title" style="margin-right: 10px;" >{{ unitName.replace(/_/g, " ") }}</h2>
                  <div class="hub-status" style="margin-right: 10px;" >
                      <span><strong>Hub Status:</strong></span>
                      <span :class="{
                            'status-light': true,
                            'status-green': getHubStatus(unitName),
                            'status-red': !getHubStatus(unitName),
                            }">
                      </span>
                  </div>
              </div>
              <div class="spool-container" style="margin-top: 15px">
                  <div v-for="(spool, index) in unit.spools" style="margin-top: 0px;"
                       :key="index"
                       class="spool-card"
                       @click="openChangeSpoolDialog(spool, index)">
                      <h3>
                          <span :class="{
                                'status-light': true,
                                'status-not-ready': determineStatus(spool) === 'Not Ready',
                                'status-ready': determineStatus(spool) === 'Ready',
                                'status-in-tool': determineStatus(spool) === 'In Tool',
                            }">
                          </span>
                          {{ spool.laneName }}
                      </h3>
                      <div class="lane-info">
                          <div class="filament-reel" style="padding: 1rem">
                              <svg viewBox="0 0 235 500" preserveAspectRatio="xMinYMin meet" width="23.5" height="50" xml:space="preserve" xmlns="http://www.w3.org/2000/svg">
                              <path style="stroke:#6e0b30;stroke-width:0;stroke-dasharray:none;stroke-linecap:butt;stroke-dashoffset:0;stroke-linejoin:miter;stroke-miterlimit:4;fill:#c08f4f;fill-rule:nonzero;opacity:1" vector-effect="non-scaling-stroke" transform="matrix(.58757 0 0 3.94769 197.135 250.047)" d="M0-63.27c34.925 0 63.27 28.345 63.27 63.27 0 34.925-28.345 63.27-63.27 63.27-34.925 0-63.27-28.345-63.27-63.27 0-34.925 28.345-63.27 63.27-63.27z" />
                              <path style="stroke:#6e0b30;stroke-width:0;stroke-dasharray:none;stroke-linecap:butt;stroke-dashoffset:0;stroke-linejoin:miter;stroke-miterlimit:4;fill:#c08f4f;fill-rule:nonzero;opacity:1" vector-effect="non-scaling-stroke" transform="matrix(.38158 0 0 3.46232 197.135 250.047)" d="M0-63.27c34.925 0 63.27 28.345 63.27 63.27 0 34.925-28.345 63.27-63.27 63.27-34.925 0-63.27-28.345-63.27-63.27 0-34.925 28.345-63.27 63.27-63.27z" />
                              <path v-if="spool.load" class="filament-reel"
                                    :style="{
                                fill: spool.color,
                                stroke: '#000',
                                strokeWidth: 0,
                                strokeDasharray: 'none',
                                strokeLinecap: 'butt',
                                strokeDashoffset: 0,
                                strokeLinejoin: 'miter',
                                strokeMiterlimit: 4,
                                fillRule: 'nonzero',
                                opacity: 1
                                }"
                                    vector-effect="non-scaling-stroke"
                                    transform="matrix(2.07364 0 0 3.3577 117.295 250.047)"
                                    d="M-38.503-65.24h77.006V65.24h-77.006z" />
        
                              <g transform="matrix(.58757 0 0 3.94769 37.454 250.047)">
                              <filter id="a" y="-.057" height="1.114" x="-.057" width="1.272">
                              <feGaussianBlur in="SourceAlpha" stdDeviation="3" />
                              <feOffset dx="20" result="oBlur" />
                              <feFlood flood-color="rgb(0,0,0)" flood-opacity=".67" />
                              <feComposite in2="oBlur" operator="in" />
                              <feMerge>
                              <feMergeNode />
                              <feMergeNode in="SourceGraphic" />
                                    </feMerge>
                                </filter>
                              <path style="stroke:#6e0b30;stroke-width:0;stroke-dasharray:none;stroke-linecap:butt;stroke-dashoffset:0;stroke-linejoin:miter;stroke-miterlimit:4;fill:#c08f4f;fill-rule:nonzero;opacity:1;filter:url(#a)" vector-effect="non-scaling-stroke" d="M0-63.27c34.925 0 63.27 28.345 63.27 63.27 0 34.925-28.345 63.27-63.27 63.27-34.925 0-63.27-28.345-63.27-63.27 0-34.925 28.345-63.27 63.27-63.27z" />
                              <path style="stroke:#6e0b30;stroke-width:0;stroke-dasharray:none;stroke-linecap:butt;stroke-dashoffset:0;stroke-linejoin:miter;stroke-miterlimit:4;fill:#c08f4f;fill-rule:nonzero;opacity:1" vector-effect="non-scaling-stroke" transform="scale(.41452)" d="M0-63.27c34.925 0 63.27 28.345 63.27 63.27 0 34.925-28.345 63.27-63.27 63.27-34.925 0-63.27-28.345-63.27-63.27 0-34.925 28.345-63.27 63.27-63.27z" />
                            </g>
                            </svg>
                          </div>
                          <div>
                              <p v-if="spool.material">
                                  {{ spool.material }}
                              </p>
                              <p v-if="spoolWeight(spool)">
                                  {{ spoolWeight(spool) }}
                              </p>
                              <p>
                                  <!--{{ determineStatus(spool) }}-->
                              </p>
                          </div>
                      </div>
                  </div>
              </div>
          </div>
      </panel>
    <afc-change-spool-dialog
      :show-dialog="showChangeSpoolDialog"
      :index="index"
      :lane-data="selectedLane"
      @close="showChangeSpoolDialog = false"
      @fetch-spool="fetchSpoolData"
    />
  </div>
</template>

<script lang="ts">
import { Component, Mixins } from "vue-property-decorator";
import BaseMixin from "@/components/mixins/base";
import Panel from "@/components/ui/Panel.vue";
import { mdiAdjust, mdiRefresh, mdiDotsVertical } from "@mdi/js";
import AfcChangeSpoolDialog from "@/components/dialogs/AfcChangeSpoolDialog.vue";
    import BoxTurtleIcon from "@/components/ui/BoxTurtleIcon.vue";
    import NightOwlIcon from "@/components/ui/NightOwlIcon.vue";

@Component({
  components: {
    Panel,
    AfcChangeSpoolDialog,
        BoxTurtleIcon,
        NightOwlIcon,
  },
})
export default class AfcPanel extends Mixins(BaseMixin) {
  mdiAdjust = mdiAdjust;
  mdiRefresh = mdiRefresh;
  mdiDotsVertical = mdiDotsVertical;
  mainsailIconSwitch: boolean = true;
  klipperScreenIconSwitch: boolean = false;
  spoolManIconSwitch: boolean = false;
  noIconSwitch: boolean = false;

  showChangeSpoolDialog = false;
  selectedLane: any = null; // This will hold data of the clicked lane

  spoolData: any[] = [];
  intervalId: number | null = null;
  systemData: any = null;
  isPanelOpen: number[] = [0];
  currentPage: number = 0;
  spoolsPerPage: number = 4;
  index: number = 0;
  unitsData: any = {};

  private lastValidData: any = null;

  async mounted() {
    await this.fetchSpoolData();
    this.intervalId = setInterval(this.fetchSpoolData, 5);
  }

  beforeDestroy() {
    if (this.intervalId) {
      clearInterval(this.intervalId);
    }
  }

  fetchSpoolData() {
    const afcData = this.$store.state.printer.AFC
      ? JSON.parse(JSON.stringify(this.$store.state.printer.AFC))
      : null;

    if (afcData) {
      this.spoolData = this.extractLaneData(afcData);
      this.unitsData = this.groupByUnit(this.spoolData);
      this.systemData = afcData.system || {};
      for (const unitName in afcData) {
        if (afcData.hasOwnProperty(unitName) && unitName !== "system") {
          if (this.unitsData[unitName]) {
            this.unitsData[unitName].system = afcData[unitName].system || {};
          }
        }
      }
    } else {
      this.spoolData = [];
      this.unitsData = {};
      this.systemData = {};
    }
  }

  extractLaneData(spools: any) {
    const lanes = [];
    if (spools && typeof spools === "object") {
      for (const unitName in spools) {
        if (spools.hasOwnProperty(unitName) && unitName !== "system") {
          const unit = spools[unitName];
          for (const laneKey in unit) {
            if (
              unit.hasOwnProperty(laneKey) &&
              typeof unit[laneKey] === "object" &&
              laneKey !== "system"
            ) {
              const laneData = unit[laneKey];
              laneData.unitName = unitName;
              laneData.laneName = laneKey;
              lanes.push(laneData);
            }
          }
        }
      }
    }
    return lanes;
  }

  private groupByUnit(spoolData: any[]) {
    const units: any = {};
    spoolData.forEach((spool) => {
      const unitName = spool.unitName;
      if (!units[unitName]) {
        units[unitName] = { spools: [] };
      }
      units[unitName].spools.push(spool);
    });
    return units;
  }

  openChangeSpoolDialog(spool: any, index: number) {
    this.selectedLane = { spool, laneName: spool.laneName };
    this.showChangeSpoolDialog = true;
  }

  getHubStatus(unitName) {
    if (this.unitsData[unitName]?.system?.hub_loaded !== undefined) {
      return this.unitsData[unitName].system.hub_loaded;
    }
    return this.systemData?.hub_loaded || false;
  }
  getType(unitName) {
      if (this.unitsData[unitName]?.system?.type !== undefined) {
          return this.unitsData[unitName].system.type;
      }
  }
  get toolStartSensorStatus() {
    return this.systemData?.extruders?.extruder?.tool_start_sensor || false;
  }

  get BufferStatus() {
      return this.systemData?.extruders?.extruder?.buffer_status;
  }

  spoolWeight(spool: any) {
    const weight = parseInt(spool.weight, 10);
    return weight ? `${weight} g` : '';
  }

  private determineStatus(spool: any) {
    if (spool.load && spool.prep) {
      if (this.systemData && this.systemData.current_load === spool.laneName) {
        return "In Tool";
      }
      return "Ready";
    }
    return "Not Ready";
  }

  private onIconStyleChange(selectedStyle: string) {
    this.mainsailIconSwitch = false;
    this.klipperScreenIconSwitch = false;
    this.spoolManIconSwitch = false;
    this.noIconSwitch = false;

    // Set the selected one to true
    if (selectedStyle === "mainsail") {
      this.mainsailIconSwitch = true;
    } else if (selectedStyle === "klipperscreen") {
      this.klipperScreenIconSwitch = true;
    } else if (selectedStyle === "spoolman") {
      this.spoolManIconSwitch = true;
    } else if (selectedStyle === "none") {
      this.noIconSwitch = true;
    }
  }
}
</script>

<style scoped>
.afc-panel {
  background-color: #1e1e1e;
  color: #ffffff;
  margin-bottom: 16px;
}

.afc-panel-wrapper {
  margin-bottom: 24px;
}

.v-card-title {
  font-weight: bold;
  font-size: 1.5em;
  text-align: center;
  padding-bottom: 6px;
}
    .printer-section {
        display: flex;
        align-items: center;
        margin-bottom: 1px;
        justify-content: left;
        vertical-align: middle
    }
    .printer-info {
        display: flex;
        flex-wrap: nowrap;
        justify-content: center;
    }
    .unit-section {
        
        margin-bottom: 6px;
        background-color: #2e2e2e;
    }

    .unit-title {
        display: flex;
        align-items: center;
        margin-bottom: 1px;
        justify-content: left;
        vertical-align: middle
    }

.spool-container {
        display: flex;
        flex-wrap: nowrap;
        justify-content: center;
        gap: 2px;
        margin-top: 0px;
}

.spool-card {
        border-style: solid;
        border-width: 2px;
        border-color: black;
        padding: 8px;
        max-width: 180px;
        min-height: 117px;
        width: 100%;
        position: relative;
        cursor: hand;
        transition: box-shadow 0.3s;
        margin-bottom: 0px;
}


.status-light {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  display: inline-block;
  margin-left: 5px;
}

.status-green {
  background-color: rgb(24, 177, 24);
}

.status-red {
  background-color: red;
}

.hub-status {
  text-align: center;
}

.tool-status {
  text-align: center;
  margin-left: 15px;
}

.status-not-ready {
  background-color: red;
}

.status-ready {
  background-color: rgb(26, 230, 26);
}

.status-in-tool {
  background-color: rgb(6, 197, 245);
}

.spool-ks {
  flex: 1;
  padding: 15px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.spool-ks h4 {
  margin-bottom: 10px;
  color: #333;
}

.spool-ks p {
  margin: 5px 0;
  color: #666;
}
</style>
