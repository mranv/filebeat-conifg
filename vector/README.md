I'll help you create an equivalent Vector configuration that matches your existing Logstash setup exactly. Given the files in your directory and the Logstash configuration, here's the Vector equivalent:



To implement this, follow these steps:

1. Install Vector:
```bash
curl -1sLf 'https://repositories.timber.io/public/vector/cfg/setup/bash.deb.sh' | sudo -E bash
sudo apt-get install vector
```

2. Create necessary directories:
```bash
sudo mkdir -p /etc/vector/certs
sudo mkdir -p /var/lib/vector
```

3. Set up the OpenSearch template:
```bash
sudo cp wz-os-4.x-2.x-template.json /etc/vector/templates/
```

4. Set up permissions:
```bash
# Add vector user to wazuh group
sudo usermod -a -G wazuh vector

# Set proper permissions for certificates
sudo chown -R vector:vector /etc/vector
sudo chmod 750 /etc/vector/certs
```

5. Set environment variables (securely):
```bash
sudo tee /etc/default/vector > /dev/null << 'EOF'
VECTOR_OS_USER=your_opensearch_username
VECTOR_OS_PASS=your_opensearch_password
OPENSEARCH_ADDRESS=https://your.opensearch.host:9200
EOF
sudo chmod 600 /etc/default/vector
```

6. Enable and start Vector:
```bash
sudo systemctl enable vector
sudo systemctl start vector
```

Key differences from your Logstash configuration:

1. More secure by default:
   - Strict file permissions
   - No keystore needed (uses environment variables)
   - Built-in TLS verification

2. Better performance:
   - Lower memory usage
   - More efficient JSON parsing
   - Automatic back-pressure handling

3. Enhanced monitoring:
   - Built-in metrics endpoint
   - Detailed error logging
   - Component-level health checks

Would you like me to explain any part of the configuration or modify it to better match your specific needs?