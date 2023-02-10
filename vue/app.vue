<template>
  <vuci-form :uci-config="uci.config" style="display: flex; flex-direction: row; flex-wrap: wrap; gap: 5px;">
    <vuci-named-section title="MQTT Broker" :name="uci.section" v-slot="{ s }" style="flex-grow: 1;">
      <vuci-form-item-switch
        :uci-section="s"
        label="Enable"
        name="enabled"
      />
      <vuci-form-item-input
        :uci-section="s"
        label="Local port"
        name="local_port"
        rules="port"
      />
      <vuci-form-item-switch
        :uci-section="s"
        label="Enable remote access"
        name="enable_ra"
      />
    </vuci-named-section>
    <vuci-named-section title="Broker Settings" :name="uci.section" v-slot="{ s }" style="flex-grow: 1;">
      <a-tabs>
        <a-tab-pane key="security" tab="Security">
          <vuci-form-item-switch
            label="Use TLS/SSL"
            :uci-section="s"
            name="use_tls_ssl"
          />
          <vuci-form-item-select
            :uci-section="s"
            label="TLS Type"
            name="tls_type"
            :options="tls_type"
            initial="cert"
            depend="use_tls_ssl == 1"
          />
          <vuci-form-item-upload
            :uci-section="s"
            label="CA file"
            name="ca_file"
            depend="use_tls_ssl == 1 && tls_type == 'cert'"
          />
          <vuci-form-item-upload
            :uci-section="s"
            label="Cert file"
            name="cert_file"
            depend="use_tls_ssl == 1 && tls_type == 'cert'"
          />
          <vuci-form-item-upload
            :uci-section="s"
            label="Key file"
            name="key_file"
            depend="use_tls_ssl == 1 && tls_type == 'cert'"
          />
          <vuci-form-item-select
            :uci-section="s"
            label="TLS version"
            name="tls_version"
            :options="tls_version"
            initial="all"
            depend="use_tls_ssl == 1 && tls_type == 'cert'"
          />
          <vuci-form-item-input
            :uci-section="s"
            label="Pre-Shared-Key"
            name="psk"
            depend="use_tls_ssl == 1 && tls_type == 'psk'"
          />
          <vuci-form-item-input
            :uci-section="s"
            label="Identity"
            name="identity"
            depend="use_tls_ssl == 1 && tls_type == 'psk'"
          />
        </a-tab-pane>
        <a-tab-pane key="miscellaneuos" tab="Miscellaneous">
          <vuci-form-item-upload
            :uci-section="s"
            label="ACL file"
            name="acl_file_path"
          />
          <vuci-form-item-upload
            :uci-section="s"
            label="Password file"
            name="password_file"
          />
          <vuci-form-item-switch
            label="Persistence"
            :uci-section="s"
            name="persistence"
          />
          <vuci-form-item-switch
            label="Allow Anonymous"
            :uci-section="s"
            name="anonymous_access"
          />
        </a-tab-pane>
      </a-tabs>
    </vuci-named-section>
    <div class="break" />
  </vuci-form>
</template>

<script>
export default {
  data () {
    return {
      uci: {
        config: 'mosquitto',
        section: 'mqtt'
      },
      tls_type: [
        ['cert', 'Certificate based'],
        ['psk', 'Pre-Shared-Key base']
      ],
      tls_version: [
        ['all', 'Support all'],
        ['tlsv1', 'TLSV1'],
        ['tlsv1.1', 'TLSV1.1'],
        ['tlsv1.2', 'TLSV1.2']
      ]
    }
  }
}
</script>

<style>
.break {
  flex-basis: 100%;
  height: 0
}
.ant-row.ant-form-item {
  flex-grow: 1
}
</style>
