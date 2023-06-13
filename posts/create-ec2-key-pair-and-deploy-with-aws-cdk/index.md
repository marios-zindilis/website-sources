---
title: Create an EC2 key pair with ssh-keygen and deploy with AWS CDK
description: How to create an EC2 key pair with ssh-keygen and deploy with AWS CDK
first-published: 2023-06-13
tags:
- AWS EC2
- AWS CDK
---

You can create a key pair that will be used to SSH into EC2 instances with:

```
ssh-keygen -m PEM -b 4096 -f ~/.ssh/my-key-pair -t rsa
```

This will create two files in `~/.ssh/`, one with the private key named `my-key-pair` and one with the public key named
`my-key-pair.pub`. Parameters:

* `-m`: The key format.
* `-t`: The key type. At the time of this writing on 2023-06-13, EC2 supports RSA and ED25519 keys.
* `-b`: The key bits. 
* `-f`: The file name in which the key will be stored. Two files will be generated, one for the private and one for the
    public key.

You can use the public key to create an EC2 key pair and associate it with your EC2 instances. Here's a minimal CDK code
snippet for this:

```typescript
import { Stack, StackProps } from 'aws-cdk-lib';
import * as ec2 from 'aws-cdk-lib/aws-ec2';
import { Construct } from 'constructs';

export class MyStack extends Stack {
  constructor(scope: Construct, id: string, props?: StackProps) {
    super(scope, id, props);

    const cfnKeyPair = new ec2.CfnKeyPair(this, 'MyCfnKeyPair', {
      keyName: 'keyName',
      publicKeyMaterial: [
        'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCtS/sY98Yk6RqJXrWQIqMrRoesRKTI0s6xRUlSPJzx7G8kbWKEH1YS+kE0xFOfdbo/MpXpU',
        'yFf9vTIKS5HEG5ZKhFnLpbh3fBBfFmkFNazJcxpyu4yGQyy8SEhavM8xMl1NCpIhBmg8fccn78FwHVjrwBDaXlLkCkHkQf5AM+Fgx2lEOuSNz',
        '4NmIvDBAEzJi8gixgKlZM5wnyEOHXyUQ04Xs+vS6RHLxmBQ90ncmMga9FhflqfmSC8r/1uMVQYgW+8/pXOGvbMRmdy9zxxnIz6EBcNtAyWhGO',
        'sWB743fdXpCpbIqtiMXImkpjnItU15ar9ij+vkgB5nKBBqFbIvlQ0IKYZ5VJxZMFlpRNZAVyEDedcDWSvc8As5APYau/UgdEv73ingEZpqZR5',
        'VcpKQfP4F3psgHtIO+cyPvKss0Q0vKPMwmpl7z5RRcbKxWGXizsQ+B9kvVs3HzK8gu4qaDW1RbEyWkdIzOkV+ovnhqzbn9o6078hkdIU62wix',
        'k7fI9ugiOEFLoTiiAUo2H/nQ+Z06I+rxrOgF3ucGpBmAm6VaIO0upjysbKL+g05WRj5BKsHp2a2DfMlzp+TcDbpMcy/4YXYwA+BGIilIKeFbR',
        'AkWDT6MP/mLfh0ud4+xZpdymS1Qvq4AzasRVQatVWZpaVWOpGzjF5KJkzhWz4DHAnL5Q== m@e'
      ].join('')
    });
  }
}
```

References:

* [CfnKeyPair (construct)](https://docs.aws.amazon.com/cdk/api/v1/docs/@aws-cdk_aws-ec2.CfnKeyPair.html) in the CDK documentation.
* [Amazon EC2 key pairs and Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the EC2 user guide.

Image credit: *Kylemore Abbey*, by Lode Van de Velde. In the Public Domain.
[Source](https://www.publicdomainpictures.net/en/view-image.php?image=69064&picture=kylemore-abbey).
