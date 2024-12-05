<template>
    <div class="calendar-config">
        <el-form label-width="90px">
            <el-form-item label="选择字体">
                <el-select v-model="localCalendarConfig.selectedFont" @change="changeFont" placeholder="选择字体">
                    <el-option
                        v-for="font in fonts"
                        :key="font.value"
                        :label="font.label"
                        :value="font.value">
                    </el-option>
                </el-select>
            </el-form-item>

            <el-form-item label="总天数">
                <el-input-number v-model="localCalendarConfig.totalDays" :min="28" :max="31" @change="updateConfig" />
            </el-form-item>

            <el-form-item label="一号星期">
                <el-select v-model="localCalendarConfig.firstDayOfWeek" @change="updateConfig">
                    <el-option v-for="day in weekDays" :key="day.value" :label="day.label" :value="day.value" />
                </el-select>
            </el-form-item>

            <el-form-item label="特殊日期">
                <el-button @click="addSpecialDay" style="margin: 0 0 20px 0;">添加特殊日期</el-button>
                <div v-for="(day, index) in localCalendarConfig.specialDays" :key="index" class="special-day-item">
                    <el-input-number v-model="day.day" :min="1" :max="localCalendarConfig.totalDays"
                        controls-position="right" placeholder="日期" style="width: 250px;" />
                    <el-input v-model="day.nickname" placeholder="别名" />
                    <el-select v-model="day.className">
                        <el-option value="" label="普通日期" />
                        <el-option value="holiday-color" label="法定节假日" />
                        <el-option value="cover-shift-color" label="补班日" />
                    </el-select>
                    <el-button @click="removeSpecialDay(index)" type="danger" :icon="Delete">删除</el-button>
                    <el-divider />
                </div>
            </el-form-item>

            <el-form-item label="颜色配置">
                <div class="color-config">
                    <div v-for="(color, key) in localColorConfig" :key="key">
                        <span>{{ getColorLabel(key) }}</span>
                        <el-color-picker v-model="localColorConfig[key]" @change="updateColorConfig" />
                    </div>
                </div>
            </el-form-item>
            <el-button type="primary" @click="exportCalendar">导出日历为 PNG</el-button>
        </el-form>
    </div>
</template>

<script setup>
import {
    Delete,
} from '@element-plus/icons-vue'
import { ref, watch } from 'vue'
import html2canvas from 'html2canvas'

const props = defineProps({
    calendarConfig: { type: Object, required: true },
    colorConfig: { type: Object, required: true }
})

const emit = defineEmits(['update:calendarConfig', 'update:colorConfig'])

const localCalendarConfig = ref({ ...props.calendarConfig })
const localColorConfig = ref({ ...props.colorConfig })
const localSelectedFont = ref({ ...props.selectedFont }); // 默认字体

const weekDays = [
    { label: '一', value: 0 },
    { label: '二', value: 1 },
    { label: '三', value: 2 },
    { label: '四', value: 3 },
    { label: '五', value: 4 },
    { label: '六', value: 5 },
    { label: '日', value: 6 }
]
// 字体选择项
const fonts = [
    { label: '寒蝉全圆体', value: '寒蝉全圆体' },
    { label: '霞鹜文楷', value: 'LXGWWenKai' },
    { label: '阿里巴巴大楷', value: 'Alimama DongFangDaKai' },
    { label: '寒蝉活仿楷', value: 'ChillHuoFangKai_F' },
    { label: '天王星像素', value: 'Uranus Pixel 11Px' },
    { label: '猫啃珠圆体', value: 'MaokenZhuyuanTi' },
    { label: '观致8像素', value: 'GuanZhi 8px' },
    { label: '得意黑', value: 'Smiley Sans Oblique' },
];




const changeFont = (font) => {
    console.log("切换字体", font);
    localCalendarConfig.value.selectedFont = font;
    updateConfig();
    
}

const getColorLabel = (key) => {
    const labels = {
        weekendColor: '周末颜色',
        dayOffColor: '节假日文字颜色',
        dayOffBgColor: '节假日背景颜色',
        coverShiftColor: '补班日文字颜色',
        coverShiftBgColor: '补班日背景颜色'
    }
    return labels[key]
}

const addSpecialDay = () => {
    localCalendarConfig.value.specialDays.push({
        day: 1,
        nickname: '',
        className: ''
    })
}

const removeSpecialDay = (index) => {
    localCalendarConfig.value.specialDays.splice(index, 1)
}

const updateConfig = () => {
    emit('update:calendarConfig', localCalendarConfig.value)
}

const updateColorConfig = () => {
    emit('update:colorConfig', localColorConfig.value)
}
const exportCalendar = () => {
    const calendarElement = document.getElementById('print-box') // 获取日历的 DOM 元素
    html2canvas(calendarElement, {
        scale: 4,
        useCORS: true
    }).then(canvas => {
        const link = document.createElement('a');
        link.href = canvas.toDataURL('image/png');
        link.download = 'calendar.png';
        link.click(); // 自动触发下载
    });
}

watch(() => props.calendarConfig, (newVal) => {
    localCalendarConfig.value = { ...newVal }
}, { deep: true })

watch(() => props.colorConfig, (newVal) => {
    localColorConfig.value = { ...newVal }
}, { deep: true })
</script>

<style scoped>
.special-day-item {
    display: flex;
    margin-bottom: 10px;
    gap: 10px;
    align-items: center;
    flex-wrap: nowrap;
    flex-direction: column;
    justify-content: flex-start;
    align-content: flex-start;
}

.color-config {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.color-config>div {
    display: flex;
    align-items: center;
    gap: 10px;
}

.el-form-item {
    margin-bottom: 15px;
}

.calendar-config>>>.el-form-item__content {
    align-items: flex-start;
    display: flex;
    flex: 1;
    flex-wrap: nowrap;
    font-size: var(--font-size);
    line-height: 32px;
    min-width: 0;
    position: relative;
    flex-direction: column;
    justify-content: center;
}
</style>