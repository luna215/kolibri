<template>

  <KNavbar>
    <KNavbarLink
      v-if="canManageContent"
      :title="coreString('channelsLabel')"
      :link="$router.getRoute('MANAGE_CONTENT_PAGE')"
    >
      <mat-svg name="apps" category="navigation" />
    </KNavbarLink>
    <KNavbarLink
      v-if="isSuperuser"
      :title="$tr('permissionsLabel')"
      :link="$router.getRoute('MANAGE_PERMISSIONS_PAGE')"
    >
      <mat-svg name="https" category="action" />
    </KNavbarLink>
    <KNavbarLink
      :title="$tr('infoLabel')"
      :link="$router.getRoute('DEVICE_INFO_PAGE')"
    >
      <mat-svg name="perm_device_information" category="action" />
    </KNavbarLink>
    <KNavbarLink
      :title="$tr('settingsLabel')"
      :link="$router.getRoute('DEVICE_SETTINGS_PAGE')"
    >
      <mat-svg name="settings" category="action" />
    </KNavbarLink>
  </KNavbar>

</template>


<script>

  import { mapGetters } from 'vuex';
  import KNavbar from 'kolibri.coreVue.components.KNavbar';
  import KNavbarLink from 'kolibri.coreVue.components.KNavbarLink';
  import commonCoreStrings from 'kolibri.coreVue.mixins.commonCoreStrings';

  export default {
    name: 'DeviceTopNav',
    components: {
      KNavbar,
      KNavbarLink,
    },
    mixins: [commonCoreStrings],
    computed: {
      ...mapGetters(['canManageContent', 'isSuperuser']),
    },
    $trs: {
      permissionsLabel: 'Permissions',
      infoLabel: 'Info',
      settingsLabel: 'Settings',
    },
  };

</script>


<style lang="scss" scoped></style>
