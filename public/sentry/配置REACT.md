#### 配置 REACT

>This is a quick getting started guide. For in-depth instructions on integrating Sentry with React, view our complete documentation.
To use Sentry with your React application, you will need to use @sentry/browser (Sentry’s browser JavaScript SDK).
On its own, @sentry/browser will report any uncaught exceptions triggered from your application.

>If you’re using React 16 or above, Error Boundaries are an important tool for defining the behavior of your application in the face of errors. Be sure to send errors they catch to Sentry using Sentry.captureException. This is also a great opportunity to collect user feedback by using Sentry.showReportDialog.

>One important thing to note about the behavior of error boundaries in development mode is that React will rethrow errors they catch. This will result in errors being reported twice to Sentry with the above setup, but this won’t occur in your production build.

>要将Sentry与React应用程序一起使用，您需要使用@ sentry / browser（Sentry的浏览器JavaScript SDK）。

>@ sentry / browser本身会报告从您的应用程序触发的任何未捕获的异常。

>如果您使用的是React 16或更高版本，则错误边界是用于在面临错误时定义应用程序行为的重要工具。请务必使用Sentry.captureException将他们捕获的错误发送给Sentry。这也是使用Sentry.showReportDialog收集用户反馈的绝佳机会。

>注意

>关于开发模式中错误边界行为的一个重要注意事项是React将重新抛出它们捕获的错误。这将导致使用上述设置向Sentry报告两次错误，但这不会在生产版本中发生。

```ts
import * as Sentry from '@sentry/browser';

// Sentry.init({
//  dsn: ""
// });
// should have been called before using it here
// ideally before even rendering your react app

class ExampleBoundary extends Component {
    constructor(props) {
        super(props);
        this.state = { error: null, eventId: null };
    }

    componentDidCatch(error, errorInfo) {
      this.setState({ error });
      Sentry.withScope(scope => {
          scope.setExtras(errorInfo);
          const eventId = Sentry.captureException(error);
          this.setState({eventId})
      });
    }

    render() {
        if (this.state.error) {
            //render fallback UI
            return (
              <a onClick={() => Sentry.showReportDialog({ eventId: this.state.eventId })}>Report feedback</a>
            );
        } else {
            //when there's not an error, render children untouched
            return this.props.children;
        }
    }
}
```