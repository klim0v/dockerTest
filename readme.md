## Docker installation

<ol>
<li>Install <a href="https://docs.docker.com/engine/installation/" rel="nofollow">docker</a> and <a href="https://docs.docker.com/compose/install/" rel="nofollow">docker-compose</a> to your system</li>
<li>Add <code>127.0.0.1 vitbiomed.loc</code> to your <code>/etc/hosts</code> file</li>
<li>Copy <code>.env.docker</code> to <code>.env</code> in the project root</li>
<li>Generate a Docker bundle from the Compose file <code>docker-compose build</code></li>
<li>Create and start containers with demon mode <code>docker-compose up -d</code></li>
<li>Install composer dependencies <code>docker-compose exec -u user web composer install</code></li>
<li>Set the application key <code>docker-compose exec -u user web php artisan key:generate</code></li>
<li>Run the database migrations with <code>docker-compose exec -u user web php artisan migrate</code></li>
<li>Create symlink for storage <code>cd public && ln -s ../storage/app/public storage </code></li>
<li>Seed the database with r    ecords <code>docker-compose exec -u user web php artisan db:seed --class=AdminSeeder</code></li>
<li>That's all - your application is accessible on <a href="http://vitbiomed.loc" rel="nofollow">http://vitbiomedplus.loc</a></li>
<li>Use command <code>docker-compose exec <b>-u user</b> web php artisan</code> for non-root user</li>
</ol>