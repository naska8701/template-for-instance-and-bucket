# template-for-instance-and-bucket
This cloudformation template creates a vpc, subnet, internet gateway, route table and all association, EC2 instance and a S3 bucket
AWSTemplateFormatVersion: 2010-09-09
Description: This template creates 2 ec2 instances with predefined parameters and a s3 bucket
Resources:
  demovpc:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: --.--.--.--/--
      EnableDnsSupport: true
      Tags:
        - Key: Name
          Value: demo

  demosubnet:
    Type: AWS::EC2::Subnet
    Properties:
      AvailabilityZone: us-east-1a
      VpcId:
        Ref: demovpc
      CidrBlock: --.--.--.--/--
      Tags:
        - Key: Name
          Value: demosubnet
  
  demoigw:
    Type: AWS::EC2::InternetGateway
    Properties:
      Tags:
        - Key: Name
          Value: igw
  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: 
        Ref: demovpc
      InternetGatewayId:
        Ref: demoigw

  myrt:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:  
        Ref: demovpc

  mySubnetRouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId:
        Ref: demosubnet
      RouteTableId:
        Ref: myrt

  demoinstance:
    Type: AWS::EC2::Instance
    Properties:
      KeyName: 'namekeypair'
      DisableApiTermination: 'False'
      ImageId: "ami-0022f774911c1d690"
      InstanceType: 't2.micro'
      BlockDeviceMappings:
        -
          DeviceName: /dev/xvda
          Ebs:
            VolumeType: gp2            
            DeleteOnTermination: true
            VolumeSize: 8
      Tags:
        - Key: Name
          Value: Demo-EC2

  demobucket20220630:
    Type: AWS::S3::Bucket
    Properties: 
      AccessControl: Private
      BucketName: demo-bucket-2022-06-30
      Tags:
        - Key: Name
          Value: demo-s3-buket
