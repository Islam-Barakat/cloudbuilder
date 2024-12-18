pipeline {
    agent any
    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }
    parameters {
        string(name: 'VAR_FILE', defaultValue: 'deploy_variables.properties', description: 'Path to properties file')
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Islam-Barakat/cloudbuilder.git'
            }
        }
        stage('Read Properties') {
            steps {
                script {
                    // Read the properties file
                    def props = readProperties file: "${params.VAR_FILE}"

                    // Assign the values to environment variables
                    env.ESXI_HOST = props['ESXI_HOST']
                    env.OVA_PATH = props['OVA_PATH']
                    env.VM_NAME = props['VM_NAME']
                    env.VM_NETWORK = props['VM_NETWORK']
                    env.ESXI_USER = props['ESXI_USER']
                    env.ESXI_PASSWORD = props['ESXI_PASSWORD']
                    env.STATIC_IP = props['STATIC_IP']
                    env.NETMASK = props['NETMASK']
                    env.GATEWAY = props['GATEWAY']
                    env.DNS = props['DNS']
                    env.NTP = props['NTP']
                    env.DATASTORE = props['DATASTORE']
                }
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                ansiblePlaybook(
                    playbook: 'deploy_cloudbuilder.yml',
                    inventory: 'inventory.ini',
                    extraVars: [
                        esxi_host: "${env.ESXI_HOST}",
                        ova_path: "${env.OVA_PATH}",
                        vm_name: "${env.VM_NAME}",
                        esxi_user: "${env.ESXI_USER}",
                        esxi_password: "${env.ESXI_PASSWORD}",
                        esxi_network: "${env.VM_NETWORK}",
                        static_ip: "${env.STATIC_IP}",
                        netmask: "${env.NETMASK}",
                        gateway: "${env.GATEWAY}",
                        dns: "${env.DNS}",
                        ntp: "${env.NTP}",
                        esxi_datastore: "${env.DATASTORE}"
                    ]
                )
            }
        }
    }
}

