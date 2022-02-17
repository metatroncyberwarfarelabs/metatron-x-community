Metatron Sensors
--------------

  * [`metatron-connect-kafka`](https://github.com/metatroncyberwarfare/metatron-connect-kafka): Provides integration between [Zeek](https://zeek.org/) and Kafka. A metatron connect kafka plugin that sends logging output to Kafka.  This provides a convenient means for tools in the Hadoop ecosystem, such as Storm, Spark, and others to process the data generated by Zeek.

  * [`fastcapa`](fastcapa/): Performs fast network packet capture by leveraging Linux kernel-bypass and user space networking technology.  The probe will bind to a network interface, capture network packets, and send the raw packet data to Kafka.  This provides a scalable mechanism for ingesting high-volumes of network packet data.

  * [`pycapa`](pycapa/): Performs lightweight network packet capture, retrieves network packets from Kafka, generates `libpcap`-compliant files, and enables integration with third-party tools like Wireshark.