

# Wordpress Reset Password

This pipeline automates the process of resetting a user's password in a WordPress database. It does this by running two stages:

1. **Check MySQL client**: This stage checks if the MySQL client is installed on the Jenkins server and installs it if it is not already installed. This stage is only supported on Linux operating systems.
2. **Reset password**: This stage generates a new password for the specified user, hashes it using MD5, and then updates the user's password in the WordPress database.

## Usage

To use this pipeline, you need to provide the following parameters:

- `USERNAME`: Username of the user whose password needs to be reset.
- `MYSQL_USER`: MySQL user with privileges to access the WordPress database.
- `MYSQL_PASSWORD`: Password for the MySQL user.
- `MYSQL_HOST`: Hostname or IP address of the MySQL server.
- `MYSQL_DATABASE`: Name of the WordPress database.
- `TABLE_PREFIX` (optional): Prefix of WordPress database tables. The default value is `wp_`.

Once you have provided the required parameters, run the pipeline to reset the user's password. The new password will be printed in the console output.

Note that this pipeline assumes that the MySQL client is not already installed on the Jenkins server. If the client is already installed, you can remove the `Check MySQL client` stage from the pipeline.

## Want to connect?

Feel free to contact me on [Twitter](https://twitter.com/OnlineAnto), [DEV Community](https://dev.to/antoonline/) or [LinkedIn](https://www.linkedin.com/in/anto-online) if you have any questions or suggestions.

Or just visit my [website](https://anto.online) to see what I do.
