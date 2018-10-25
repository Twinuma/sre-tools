---
title: "Cfn Cwlogs Direct Firehose"
date: 2018-10-25T15:23:35+09:00
draft: false
---

## CloudWatch LogsのデータをFirehoseを使ってS3に保存するCloudFormationのテンプレート
```
AWSTemplateFormatVersion: '2010-09-09'
Description: cloudwatchlogs to firehose direct (no lambda) 20180520
Parameters:
  TargetLogGroup:
    Description: 'Input target log group'
    Type: String
    Default: ''
  TargetS3bucket:
    Description: 'Input S3 bucket name to save'
    Type: String
    Default: ''
  TargetS3bucketPrefix:
    Description: 'Input prefix in S3 bucket'
    Type: String
    Default: ''
  CWlogsExpiredate:
    Description: 'Number of days to keep Cloudwatch logs'
    Type: Number
    Default: 60
    AllowedValues: [1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, 3653]
  S3Expiredate:
    Description: 'Number of days to keep S3 file'
    Type: Number
    Default: 365
    AllowedValues: [1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, 3653]

Resources:
  TargetS3bucket:
    Type: AWS::S3::Bucket
    DeletionPolicy: Delete
    Properties:
      BucketName: !Ref 'TargetS3bucket'
      LifecycleConfiguration:
        Rules:
          - Id: AutoDelete
            Status: Enabled
            ExpirationInDays: !Ref 'S3Expiredate'
  SubscriptionFilter:
    Type: AWS::Logs::SubscriptionFilter
    Properties:
      DestinationArn: !GetAtt 'Deliverystream.Arn'
      FilterPattern: ''
      LogGroupName: !Ref 'TargetLogGroup'
      RoleArn: !GetAtt 'LogsRole.Arn'

  LogsRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service: !Sub 'logs.${AWS::Region}.amazonaws.com'
            Action:
              - sts:AssumeRole
      Path: /
      Policies:
        - PolicyName: root
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - firehose:*
                Resource: !GetAtt 'Deliverystream.Arn'

  Deliverystream:
    DependsOn:
      - DeliveryPolicy
    Type: AWS::KinesisFirehose::DeliveryStream
    Properties:
      ExtendedS3DestinationConfiguration:
        BucketARN: !GetAtt 'TargetS3bucket.Arn'
        BufferingHints:
          IntervalInSeconds: '60'
          SizeInMBs: '50'
        CompressionFormat: GZIP
        Prefix: !Join [ "/", [ !Ref TargetS3bucketPrefix, '' ] ]
        RoleARN: !GetAtt 'DeliveryRole.Arn'
        ProcessingConfiguration:
          Enabled: 'false'

  DeliveryRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Sid: ''
            Effect: Allow
            Principal:
              Service: firehose.amazonaws.com
            Action: sts:AssumeRole
            Condition:
              StringEquals:
                sts:ExternalId: !Ref 'AWS::AccountId'
  DeliveryPolicy:
    Type: AWS::IAM::Policy
    Properties:
      PolicyName: firehose_delivery_policy
      PolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Action:
              - s3:AbortMultipartUpload
              - s3:GetBucketLocation
              - s3:GetObject
              - s3:ListBucket
              - s3:ListBucketMultipartUploads
              - s3:PutObject
            Resource:
              - !Sub 'arn:aws:s3:::${TargetS3bucket}'
              - !Sub 'arn:aws:s3:::${TargetS3bucket}*'
      Roles:
        - !Ref 'DeliveryRole'

  LogGroupFirehose:
    Type: AWS::Logs::LogGroup
    Properties:
      LogGroupName: !Sub '/aws/firehose/${Deliverystream}'
      RetentionInDays: 7
```