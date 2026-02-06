### Deploy to Server
*   - name: Deploy to staging  
      uses: appleboy/ssh-action@v0.1.6
      with:
        host: ${{ secrets.STAGING_SERVER_SSH }}
        key: ${{ secrets.STAGING_SERVER_KEY }}
        script: |
          docker pull jericho1994/echo-cicd:latest
          docker stop echo-cicd-practice || true
          docker rm echo-cicd-practice || true
          docker run -d --name echo-cicd-practice -p 3000:3000 jericho1994/echo-cicd:latest

### Notification
*   - name: Notify Slack on failure
      if: failure()
      uses: slackapi/slack-github-action@v1.23.0
      with:
        payload: '{"text":"Deployment failed!"}'
        channel: '#ci-cd-notifications'
        webhook-url: ${{ secrets.SLACK_WEBHOOK_URL }}