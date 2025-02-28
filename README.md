{
  "variables": {
    "aws_region": "us-east-1",
    "ami_name": "nginx-ami"
  },
  "builders": [
    {
      "type": "amazon-ebs",
      "region": "{{user `aws_region`}}",
      "source_ami_filter": {
        "filters": {
          "name": "amzn2-ami-hvm-*-x86_64-gp2"
        },
        "owners": ["137112412989"],
        "most_recent": true
      },
      "instance_type": "t2.micro",
      "ssh_username": "ec2-user",
      "ami_name": "{{user `ami_name`}}-{{timestamp}}"
    }
  ],
  "provisioners": [
    {
      "type": "shell",
      "inline": [
        "sudo yum update -y",
        "sudo yum install -y nginx",
        "sudo systemctl enable nginx",
        "sudo systemctl start nginx",
        "echo '<h1>Hello, World!</h1>' | sudo tee /usr/share/nginx/html/index.html"
      ]
    }
  ]
}
