<template>
    <div>
        <topNav :helpAvailable="false" />
        <Help />        
    </div>
</template>

<script>
import Help from '@/components/Help.vue'
import topNav from '@/components/TopNav.vue'
export default {
    components: { 
        Help,
        topNav
    }
}
</script>