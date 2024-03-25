To configure your WordPress application to connect to the RDS database using the endpoint provided by RDS, follow these steps:

1. **Locate RDS Endpoint:**
   - Go to the RDS dashboard in the AWS Management Console.
   - Find the RDS instance you created earlier and note down its endpoint (hostname).

2. **Access WordPress Files:**
   - Access the files of your WordPress application, either locally or via SSH if it's hosted on a server.

3. **Update WordPress Configuration:**
   - Locate the `wp-config.php` file in the root directory of your WordPress installation.
   - Open the `wp-config.php` file in a text editor.

4. **Edit Database Connection Settings:**
   - Look for the database connection settings section, which typically looks like this:

   ```php
   define( 'DB_NAME', 'database_name_here' );
   define( 'DB_USER', 'username_here' );
   define( 'DB_PASSWORD', 'password_here' );
   define( 'DB_HOST', 'localhost' );
   ```

5. **Update Database Settings:**
   - Replace the placeholder values with the actual database settings provided by RDS:
     - `DB_NAME`: Replace with the name of the database you created in RDS.
     - `DB_USER`: Replace with the username you configured for the database in RDS.
     - `DB_PASSWORD`: Replace with the password you set for the database user in RDS.
     - `DB_HOST`: Replace with the RDS endpoint hostname provided by AWS.

6. **Save Changes:**
   - Save the `wp-config.php` file after making the necessary changes.

7. **Test Connection:**
   - Restart your web server if necessary.
   - Visit your WordPress site in a web browser to ensure it's connecting to the RDS database successfully.
   - Perform any required testing or debugging to ensure that your WordPress site is functioning correctly.

By following these steps, you'll configure your WordPress application to connect to the RDS database using the endpoint provided by RDS. Make sure to keep your `wp-config.php` file secure and properly backed up.