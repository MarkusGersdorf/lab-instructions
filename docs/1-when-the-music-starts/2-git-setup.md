## Setup GitLab

Now we’ll setup our git projects to store our code and configuration. Spoiler alert: be watch out for GitOps 🦄🔥


1. Log into GitLab with your credentials. GitLab URL [here](https://<GIT_SERVER>):

    ```bash
    https://<GIT_SERVER>
    ```

    We need to create a group in GitLab as `<TEAM_NAME>`.  Click "Create a group" on the screen:
    ![gitlab-initial-login](images/gitlab-initial-login.png)

2. Put your TEAM_NAME (<TEAM_NAME>) as the group name, select Public for Visibility level, and hit Create group. This is so we can easily share code and view other teams’ activity.

    ![gitlab-create-group.png](./images/gitlab-create-group.png)

3. Now lets create the git repository that we are going to use to store out Notebooks. Hit `New project` button on the right hand side
    ![gitlab-new-project](images/gitlab-new-project.png)

4. On the new view, use `jukebox` as Project Name, select **Internal** for Visibility level, then hit Create project. Make sure the project is in the group you created previously and not the username's.
    ![gitlab-new-project](images/gitlab-new-project-2.png)

5. Now let's go back to Jupyter Notebook. On the Launcher page, click `Terminal` at the bottom left and run below commands to push the `jukebox` code to the GitLab server.

    ![jupyter-terminal.png](images/jupyter-terminal.png)

7. Let's push our code to the GitLab server. Run the below commands:

    ```bash
    cd /opt/app-root/src/jukebox
    git remote set-url origin https://<GIT_SERVER>/<TEAM_NAME>/jukebox.git
    ```

    Use your username and password when you are prompted for the password (this will be cached).

    ```bash
    git push -u origin --all
    ```

    Now we are almost there with our initial setup!

    <p class="tip">
    ⛷️ <b>TIP</b> ⛷️ - If your credentials are cached incorrectly, you can try clearing the cache using: <strong>git credential-cache exit</strong>
    </p> 