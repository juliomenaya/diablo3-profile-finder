<template>
  <div class="skills-wrapper mt-5">
    <h2 class="font-diablo">Skills</h2>
    <hr class="bg-white">

    <b-nav pills small>
      <b-nav-item :active="!isPassiveSkillsActive" @click="changeComponent('ActiveSkills')">Active</b-nav-item>
      <b-nav-item :active="isPassiveSkillsActive" @click="changeComponent('PassiveSkills')">Passive</b-nav-item>
    </b-nav>
    <!-- Con keep-alive hacemos que los recursos (sprites) del componente a ser renderizado no se carguen
     siempre que son mostrados en la pantalla. -->
    <keep-alive>
      <component :is="activeComponent" :skills="componentProps"/>
    </keep-alive>

  </div>
</template>

<script>
export default {
  name: 'HeroSkills',
  components: {
    ActiveSkills: () => import(/* webpackChunkName: "ActiveSkills" */ './ActiveSkills'),
    PassiveSkills: () => import(/* webpackChunkName: "PassiveSkills" */ './PassiveSkills')
  },
  data () {
    return {
      activeComponent: 'ActiveSkills'
    }
  },
  computed: {
    /**
   * Dinamyc props for async dynamic components
   * @returns {String}
   */
    // Con esto estamos generando "props" dinámicas
    // Si el componente es ActiveSkills pasa como props las activas, si no, las pasivas
    componentProps () {
      return this.activeComponent === 'ActiveSkills' ? this.skills.active : this.skills.passive
    },
    // Nos dice si el componente "HabilidadesPasivas" está activo o no
    isPassiveSkillsActive () {
      return this.activeComponent === 'PassiveSkills'
    }
  },
  methods: {
    changeComponent (component) {
      this.activeComponent = component
    }
  },
  props: {
    skills: {
      required: true,
      type: Object
    }
  }
}
</script>
