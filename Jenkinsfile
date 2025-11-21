pipeline {
    agent any

    stages {

        stage('Parando servicios anteriores') {
            steps {
                bat '''
                    docker compose -p practica down || exit /b 0
                '''
            }
        }

        stage('Eliminando imágenes anteriores') {
            steps {
                bat '''
                    for /f "tokens=*" %%i in ('docker images --filter "label=com.docker.compose.project=practica" -q') do (
                        docker rmi -f %%i
                    )
                    if errorlevel 1 (
                        echo No hay imagenes por eliminar
                    ) else (
                        echo Imagenes eliminadas correctamente
                    )
                '''
            }
        }

        stage('Actualizando código desde el repo') {
            steps {
                checkout scm
            }
        }

        stage('Construyendo y levantando servicio web') {
            steps {
                bat '''
                    docker compose -p practica up --build -d
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado correctamente'
        }
        failure {
            echo 'Hubo un error en la ejecución del pipeline'
        }
        always {
            echo 'Pipeline finalizado'
        }
    }
}
