node {
    stage 'Build, Test and Package'
    sh "./mvnw clean install -DskipTests"
    sh 'nohup ./mvnw spring-boot:run -Dserver.port=8989 &'
    sh "while ! httping -qc1 http://192.168.10.13:8989 ; do sleep 1 ; done"           
    sh "jmeter -Jjmeter.save.saveservice.output_format=xml -n -t src/main/resources/JMeter.jmx -l src/main/resources/JMeter.jtl"
    step([$class: 'ArtifactArchiver', artifacts: 'JMeter.jtl'])
    sh "pid=\$(lsof -i:8989 -t); kill -TERM \$pid || kill -KILL \$pid"
    }
}
