<script setup>
const props = defineProps({
    coord: {
        type: Object,
        required: true
    }
})
</script>

<template>
    <section class="section-bottom right">
        <div class="block-bottom">
            <div class="block-bottom-inner">
                <div class="block-bottom-pic pic-coords"></div>
                <div class="block-bottom-texts">
                    <div class="block-bottom-text-block">
                        <div class="block-bottom-text-block-title">
                            Longitude: {{ coord?.lon }}
                        </div>
                        <div class="block-bottom-text-block-desc">
                            Longitude: the distance of a place east or west of an imaginary line between the North Pole and the South Pole, measured in degrees.
                        </div>
                    </div>
                    <div class="block-bottom-text-block">
                        <div class="block-bottom-text-block-title">
                            Latitude: {{ coord?.lat }}
                        </div>
                        <div class="block-bottom-text-block-desc">
                            Latitude: the position north or south of the equator measured from 0° to 90°.
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>