require 'aws-sdk'

AWS_ACCESS_ID = ''
AWS_SECRET_ID = ''
AWS_REGION = 'us-east-1'
AWS_AMI = 'ami-8cfd6de5'
AWS_INSTANCE_TYPE = 'm1.small'
AWS_INSTANCE_COUNT = 1
AWS_ELASTIC_IP = ''

task :default => :start

desc 'Setup Instance for Streaming'
task :start do

  if (Date.today.strftime('%A') != 'Sunday')
    abort "today is not Sunday"
  end

  AWS.config(access_key_id: AWS_ACCESS_ID, secret_access_key: AWS_SECRET_ID, region: AWS_REGION)

  ec2 = AWS.ec2
  
  # launch instance
  instance = ec2.instances.create(:image_id => AWS_AMI, :instance_type => AWS_INSTANCE_TYPE, :count => AWS_INSTANCE_COUNT)
  puts 'Launching instance'

  # wait until instance is fully operational
  sleep 1 until instance.status != :pending
  puts "Launched instance #{instance.id}, status: #{instance.status}, public dns: #{instance.dns_name}, public ip: #{instance.ip_address}"

  # associate ip
  ip = nil
  ec2.elastic_ips.each do |eip|
    if eip.public_ip == AWS_ELASTIC_IP
      ip = eip
    end
  end

  if ip
    instance.associate_elastic_ip(ip)  
    puts 'Associating ip address'
  else
    puts 'Could not associate ip address'
  end
    
  puts 'All done.'

end

desc 'Kill Streaming Instances'
task :stop do

  if (Date.today.strftime('%A') != 'Sunday')
    abort "today is not Sunday"
  end

  AWS.config(access_key_id: AWS_ACCESS_ID, secret_access_key: AWS_SECRET_ID, region: AWS_REGION)

  ec2 = AWS.ec2

  # terminate each instance
  puts 'Terminating instances'
  ec2.instances.each  do |instance|
    instance.terminate
  end

  puts 'All done.'

end