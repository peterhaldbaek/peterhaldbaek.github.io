---
title: JSF and OnLoad for JDK 1.4
layout: post
tags: ["java"]
date: 2008-08-07
commentsUrl: http://www.briskbee.com/2008/08/jsf-and-onload-for-jdk-14.html#comment-form
redirect_from: "/posts/jsf_and_onload_for_jdk_1_4/"
---

Calling a method in a bean when a JSF page is hit is often a needed requirement when doing JSF. This is however not something you get out of the box unfortunately and you need to look at solutions outside of the scope of the JSF specification. Frameworks like [Seam](http://www.seamframework.org/) and [Shale](http://shale.apache.org/) does address this but if you only need this functionality in one or two places it might seem a little overkill to include frameworks like these. Instead I discovered the [JSF On-Load Action Listener](http://jsf-comp.sourceforge.net/components/onload/index.html) from jsf-comp which does excactly what I want.

If I use JDK 5 or later.

Which I don't.

I use 1.4 so I decided to take a quick glance at the code and realized it was not that tricky to convert into something my compiler liked. So here is the JDK 1.4 compatible version of the JSF On-Load Action Listener. Enjoy! :)

``` java
/*
 * Copyright 2004 The Apache Software Foundation.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 * 
 * --
 * 
 * $Log$
 * Revision 1.1  2005/11/07 04:23:26  arobinson74
 * Initial revision
 */
package net.sf.jsfcomp.ext.onload;

import java.io.InputStream;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Iterator;
import java.util.List;

import javax.faces.FacesException;
import javax.faces.context.FacesContext;
import javax.faces.el.MethodBinding;
import javax.faces.event.PhaseEvent;
import javax.faces.event.PhaseId;
import javax.faces.event.PhaseListener;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

/**
 * This class allows for actions to be run on the load of a JSF page. When a
 * page is requested or a page is being redirected to due to a result of JSF
 * navigation action result, this phase listener intercepts the call. Based on
 * the configuration of on load phase listener, actions can be invoked based on
 * the requested view ID.
 * &lt;p&gt;
 * The configuration of the phase listener is needed. Inside of the web.xml file
 * of the application, the onload-rules context property must be given. This
 * value should be a URL to where to find the XML file with the configuration
 * information.
 * &lt;/p&gt;
 * Example:
 * 
 * &lt;pre&gt;
 * &amp;lt;context-param&amp;gt;
 *  &amp;lt;param-name&amp;gt;onload-rules&amp;lt;/param-name&amp;gt;
 *  &amp;lt;param-value&amp;gt;/WEB-INF/onload-rules.xml&amp;lt;/param-value&amp;gt;
 * &amp;lt;/context-param&amp;gt;
 * &lt;/pre&gt;
 * 
 * &lt;p&gt;
 * The XML file must use the onload-rules.xsd schema. An example file and the
 * schema is included in the release.
 * &lt;/p&gt;
 * &lt;p&gt; 
 * The 'view-id' element of the configuration has the same functionality of the
 * NavigationHandler behavior. The view ID value of the XML element must be
 * exactly the view ID or part of the view ID followed by the '*' character. To
 * match all views, use a '*' as the view-id element value
 * &lt;/p&gt;
 * 
 * @version $Revision: 649 $
 * @author $Author: arobinson74 $
 */
public class OnLoadPhaseListener implements PhaseListener {
  private final static long serialVersionUID = 1l;
  private static Log logger = LogFactory.getLog(OnLoadPhaseListener.class);
  private List rules; // contains objects of type NavigationRule

  /**
   * Default constructor
   */
  public OnLoadPhaseListener() {
    logger.info("OnLoadPhaseListener created");
  }

  /* (non-Javadoc)
   * @see javax.faces.event.PhaseListener#afterPhase(javax.faces.event.PhaseEvent)
   */
  public void afterPhase(PhaseEvent event) {
    // nothing to do here
  }

  /* (non-Javadoc)
   * @see javax.faces.event.PhaseListener#beforePhase(javax.faces.event.PhaseEvent)
   */
  public void beforePhase(PhaseEvent event) {
    processOnLoad(event.getFacesContext(), event.getFacesContext()
     .getViewRoot().getViewId());
  }

  /* (non-Javadoc)
   * @see javax.faces.event.PhaseListener#getPhaseId()
   */
  public PhaseId getPhaseId() {
    return PhaseId.RENDER_RESPONSE;
  }

  /**
   * Allows someone to extend this class and handle any events they want to
   * accomplish before the control is handed to the navigation handler. This
   * function is only invoked when the result of the on load action is not
   * null and is not equal to the success result
   * 
   * @param requestedViewId The original requested view ID
   * @param onLoadAction The action that was invoked on load
   * @param onLoadActionResult The on load actions result
   * @return true to let the control be passed to the navigation handler,
   *         false to abort the processing and let the current (requested)
   *         view be rendered
   */
  protected boolean beforeHandleNavigation(String requestedViewId,
    String onLoadAction, String onLoadActionResult) {
    return true;
  }

  private void processOnLoad(FacesContext ctx, String viewId) {
    NavigationRule rule;

    // ensure the settings have been loaded
    initialize();

    logger.debug("Processing on load of view " + viewId);
    rule = getRule(viewId);
    if (rule == null) {
      logger.debug("No rule found for view " + viewId);
      return;
    } else
      processRule(ctx, rule, viewId);
  }

  private NavigationRule getRule(String viewId) {
    logger.debug("Looking for rule for view " + viewId);
    for (Iterator iter = rules.iterator(); iter.hasNext();) {
      NavigationRule rule = (NavigationRule) iter.next();
      String rViewId = rule.getViewId();

      // check for exact match
      logger.debug("Checking for exact match. Rule view: " + rViewId);
      if (rViewId.equals(viewId)) {
        logger.info("Found rule with exact match");
        return rule;
      }

      // see if the rule ends with a wildcard
      if (rule.getViewId().endsWith("*") == false)
        continue;

      String ruleViewPrefix = rule.getViewId();
      ruleViewPrefix = ruleViewPrefix.substring(0, ruleViewPrefix
        .length() - 1);
      logger.debug("Checking if view starts with " + ruleViewPrefix);

      if (viewId.startsWith(ruleViewPrefix) == false)
        continue;

      logger.debug("Found rule for viewId path " + rule.getViewId());
      return rule;
    }

    return null;
  }

  private void processRule(FacesContext ctx, NavigationRule rule,
    String viewId) {
    MethodBinding method = ctx.getApplication().createMethodBinding(
      rule.getAction(), new Class[0]);

    if (method == null) {
      logger.warn("No method found for rule action " + rule.getAction());
      return;
    }

    logger.debug("Invoking action: " + rule.getAction());

    Object result = method.invoke(ctx, new Object[0]);

    if (result != null &amp;&amp; (result instanceof String)) {
      if (result.equals(rule.successResult) == false) {
        // allow sub classes to abort the navigation
        if (beforeHandleNavigation(viewId, rule.getAction(),
          (String) result) == false) {
          logger
            .info("Before handle navigation has aborted the on load "
            + "navigation");
          return;
        }
        logger.debug("Calling navigation handler with result: "
          + result);

        // let the navigation handler implementation handle the rest of the load
        ctx.getApplication().getNavigationHandler().handleNavigation(
          ctx, rule.getAction(), (String) result);

        // shouldn't be necessary, but make sure the render response is called
        // if the request wasn't redirected
        if (!ctx.getResponseComplete())
          ctx.renderResponse();
      } else
        logger.debug("Result is equal to the success result. Result: "
          + rule.successResult);
    } else {
      if (logger.isDebugEnabled())
        logger
         .debug("Result from rule is null or not a string. Result: "
         + result);
    }
  }

  private void initialize() {
    if (this.rules != null)
      return;
    loadConfiguration();
  }

  private synchronized void loadConfiguration() {
    if (this.rules != null)
      return;
    this.rules = new ArrayList(); // contains objects of type NavigationRule
    FacesContext ctx = FacesContext.getCurrentInstance();
    String url = ctx.getExternalContext().getInitParameter("onload-config");

    try {
      if (url == null) {
        String msg = "The external context did not have the 'onload-rules' initialization "
          + "parameter. Please check your configuration.";
        logger.error(msg);
        throw new FacesException(msg);
      }

      logger.info("On Load configuration is being loaded from " + url);
      InputStream configStream = FacesContext.getCurrentInstance()
        .getExternalContext().getResourceAsStream(url);

      // get the XML parsing structure
      Document xml = new SAXReader().read(configStream);
      Element root = xml.getRootElement();

      // pars out the rules
      List rules = root.elements("navigation-rule");
      logger.info(rules.size() + " rule(s) have been found");

      for (Iterator iter = rules.iterator(); iter.hasNext();) {
        // for (Element rule : toGenericIterable(rules, Element.class))
        Element rule = (Element) iter.next();
        this.rules.add(new NavigationRule(rule
          .elementTextTrim("view-id"), rule
          .elementTextTrim("action"), rule
          .elementTextTrim("success-result")));
      }

      Collections.sort(this.rules);
      if (logger.isDebugEnabled()) {
        for (Iterator iter = rules.iterator(); iter.hasNext();) {
          // for (NavigationRule rule : this.rules)
          NavigationRule rule = (NavigationRule) iter.next();
          logger.debug("Rule: " + rule);
        }
      }
    } catch (Exception ex) {
      logger.error("Failed to parse onload configuration", ex);
    }
  }

  private class NavigationRule implements Comparable {
    private String action;
    private String viewId;
    private String successResult;

    NavigationRule(String viewId, String action, String successResult) {
      this.viewId = viewId;
      this.action = action;
      this.successResult = successResult;
    }

    /**
     * @return Returns the successResult.
     */
    String getSuccessResult() {
      return this.successResult;
    }

    /**
     * @return Returns the action.
     */
    String getAction() {
      return this.action;
    }

    /**
     * @return Returns the fromViewId.
     */
    String getViewId() {
      return this.viewId;
    }

    public String toString() {
      return "view-id: " + viewId + ", action: " + action + ", success-result: " + successResult;
    }

    /* (non-Javadoc)
     * @see java.lang.Comparable#compareTo(T)
     */
    public int compareTo(Object o) {
      if (o instanceof NavigationRule) {
        NavigationRule rule = (NavigationRule) o;

        // sort longest to shortest
        return viewId.compareTo(rule.getViewId()) * -1;
      }

      return -1;
    }
  }
}
```

