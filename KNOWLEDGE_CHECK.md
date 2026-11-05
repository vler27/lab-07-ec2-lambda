# Lab 07: Knowledge check

**Answer each question below it, in your own words, in one to three sentences.** Replace the `...` below **Your answer:** with your text. Questions 1 to 3 also ask for a screenshot, which goes into the folder `results/`.

Questions 1 to 3 describe **hypothetical** problems: they may not have happened to you, but they are common, and you should know where to look when they do.

## 1. EC2: why can't I log in with `ssh`?

Suppose your instance is running and has a public IP address, but `ssh` cannot connect. A common cause is the instance's security group: a firewall that decides which connections reach the instance.

**Your task:** find the place in the AWS Console that shows which connections your instance's security group lets in. Take a screenshot of it and save it as `results/security-group.png`. (The security group is still there after you terminated the instance.)

*Hint:* the lecture slides, and [class/07-ec2](https://github.com/ksiller/DS2022/blob/main/class/07-ec2/README.md), part [3. Launch an EC2 instance in the AWS Console + SSH](https://github.com/ksiller/DS2022/blob/main/class/07-ec2/README.md#3-launch-an-ec2-instance-in-the-aws-console--ssh-15-min) and [Troubleshooting](https://github.com/ksiller/DS2022/blob/main/class/07-ec2/README.md#troubleshooting).

**Your answer:** Which path did you take in the console (service → page → tab)?

The path I took in the console was EC2, then Security Groups, then Network & Security under that, then launch-wizard-1, then the Inbound rules tab. There, it shows which connections are allowed in. If ssh can’t connect, I would check there if that a rule allows SSH from my IP address, or from anywhere (0.0.0.0/0) like mine.

## 2. Lambda: why didn't my function run?

Suppose you uploaded a new book to your bucket, but several minutes later MongoDB still has no document for it. Maybe the function never started, because its trigger is missing or wrong.

**Your task:** find the place in the AWS Console that shows what starts your Lambda function. Take a screenshot of it and save it as `results/lambda-trigger.png`.

*Hint:* the class demo [A Lambda function in the AWS Console](https://github.com/ksiller/DS2022/blob/main/demos/07-lambda/README.md), steps [5. Add the S3 trigger](https://github.com/ksiller/DS2022/blob/main/demos/07-lambda/README.md#5-add-the-s3-trigger) and [8. Preview: the same steps with Chalice](https://github.com/ksiller/DS2022/blob/main/demos/07-lambda/README.md#8-preview-the-same-steps-with-chalice).

**Your answer:** Which path did you take in the console (service → page → tab)?

The path I took in the console was Lambda, then Functions, then book-analyzer-dev-analyze_new_book, then Function overview, then Triggers. The trigger is an S3 event in my bucket.  S3 starts the function. If the function never ran, this is where I’d check that the trigger exists, where it was created and showing if it was created in the right bucket.

## 3. Lambda: where do I find the error?

Suppose the function did start, but MongoDB still has no document for the book. Then the function failed while it ran, and its error message is in its log.

**Your task:** find the place in the AWS Console where your Lambda function's log is, and open the log of the run for `pg74.txt`. Take a screenshot that shows the line `pg74.txt: ... words`, and save it as `results/lambda-cloudwatch-log.png`.

*Hint:* [Part 2, Step 7](README.md#step-7-upload-a-new-book) in the README, and the class demo [A Lambda function in the AWS Console](https://github.com/ksiller/DS2022/blob/main/demos/07-lambda/README.md), step [7. Check the CloudWatch logs](https://github.com/ksiller/DS2022/blob/main/demos/07-lambda/README.md#7-check-the-cloudwatch-logs).

**Your answer:** Which path did you take in the console (service → page → tab)?

The path I took was Lambda, then Functions, then book-analyzer-dev-analyze_new_book, then Monitor tab, then View CloudWatch logs, then newest log stream. If the function failed, I’d read the newest stream here to see why.

## 4. EC2 or Lambda: a backlog or new uploads?

Which would you use, EC2 or Lambda, in each of these two cases? Explain each choice in one or two sentences.

- **Case A, a backlog:** a library gives you 5,000 more books. They are already in your bucket, and you need to analyze them once.
- **Case B, new uploads:** readers upload new books at random times, a few each day. Each book should be analyzed within a minute after its upload.

For comparison, typical times in this lab:

- **EC2** (`t3.micro`): about 20 seconds for all 100 books, plus a few minutes to start the instance and run the bootstrap. The instance costs money for every minute it runs, also while it waits.
- **Lambda** (512 MB): about 2 seconds for one book, including about 1 second to start the function the first time (a cold start). The function runs, and costs, only while a book is analyzed.

**Your answer:**

I would use EC2 For the backlog of books already in the bucket. I would first start one machine and then run analyze.py over all the books in one batch. I would terminate that when it finishes. I would use Lambda for new books that was uploaded a few at a time and each upload triggers the function right away. 
