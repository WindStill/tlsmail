Installation
------------

	# gem install tlsmail

License
-------

This library is public domain software and provided without warranty of any kind.  

Author
------

zoriorz@gmail.com, xrlange@gmail.com  

Description
-----------

This library dynamically replace net/smtp and net/pop with these in ruby 1.9 and  
  enables pop or smtp via SSL/TLS.  

  This is tested on Ruby 1.8.4, 1.8.5 and 1.9.2

Usage 
-----

###SMTP

	def send_message
	  require "time"
	  msgstr = <<END_OF_MESSAGE
	From: Your Name <you@gmail.com>
	To: Destination Address <yourfriend@gmail.com>
	Subject: test message
	Date: #{Time.now.rfc2822}
	
	test message body.
	END_OF_MESSAGE
	
	Net::SMTP.enable_tls(OpenSSL::SSL::VERIFY_NONE)
	Net::SMTP.start("smtp.gmail.com", "587", "your.domain", "username", "password", :plain) do |smtp|
	  smtp.send_message msgstr, "you@gmail.com", "yourfriend@gmail.com"
	end
	end 

###POP

	def receive_messages
	  Net::POP.enable_ssl(OpenSSL::SSL::VERIFY_NONE)
	  Net::POP.start("pop.gmail.com", "995", "username", "password") do |pop|
	    p pop.mails[0].pop
	  end
	end

###God
	
	require 'tlsmail'
	
	#Enable_tls
	Net::SMTP.enable_tls(OpenSSL::SSL::VERIFY_NONE)
	#the global email config
	God::Contacts::Email.defaults do |d|
	  d.from_email = 'noreply@example.com'
	  d.from_name = 'example_monitor'
	  d.delivery_method = :smtp
	  #smtp config
	  d.server_host = 'smtp.gmail.com'
	  d.server_port = 587
	  d.server_auth = :login
	  #smtp auth options 
	  d.server_domain = 'domain'
	  d.server_user = 'noreply@example.com'
	  d.server_password = 'password'
	end
