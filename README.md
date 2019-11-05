# Chores

1) <a href="#Weblogic-topic-consumption">Weblogic topic consumption by only one instance in the cluster using Message Driven Beans</a>











































<h2><a id="content-Weblogic-topic-consumption" class="anchor" aria-hidden="true" href="#Weblogic-topic-consumption"></a>Weblogic topic consumption by only one instance in the cluster using Message Driven Beans</h2>

<b><u>ejb-jar.xml:</u></b>

<?xml version="1.0" encoding="UTF-8"?>
<ejb-jar xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/ejb-jar_2_1.xsd"
	version="2.1">
	<enterprise-beans>	
    <message-driven>
		<ejb-name>MDBBean</ejb-name>
		<ejb-class>org.learning.MyMDB</ejb-class>            
		<transaction-type>Container</transaction-type>
			<activation-config>
				<activation-config-property>
					<activation-config-property-name>destinationType</activation-config-property-name>
					<activation-config-property-value>javax.jms.Topic</activation-config-property-value>
				</activation-config-property>
				<activation-config-property>
					<activation-config-property-name>topicMessagesDistributionMode</activation-config-property-name>
					<activation-config-property-value>One-Copy-Per-Application</activation-config-property-value>
				</activation-config-property>
			</activation-config>
	</message-driven>   	
	</enterprise-beans>	
</ejb-jar>

<b><u>weblogic-ejb-jar.xml:</u></b>

<?xml version="1.0"?>	
<weblogic-ejb-jar xmlns="http://www.bea.com/ns/weblogic/10.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.bea.com/ns/weblogic/10.0 http://www.bea.com/ns/weblogic/10.0/weblogic-ejb-jar.xsd http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/ejb-jar_3_0.xsd">
	
	<weblogic-enterprise-bean>
		<ejb-name>MDBBean</ejb-name>
		<message-driven-descriptor>
			<destination-jndi-name>jms/myTopic</destination-jndi-name>
			<provider-url>t3://<host>:<port></provider-url> <!-- t3 ot t3s -->
		</message-driven-descriptor>
	</weblogic-enterprise-bean>
</weblogic-ejb-jar>

<b><u>MyMDB.java:</u></b>

import javax.ejb.EJBException;
import javax.ejb.MessageDrivenBean;
import javax.ejb.MessageDrivenContext;
import javax.jms.Message;
import javax.jms.MessageListener;

import org.apache.logging.log4j.Logger;

public class MyMDB implements MessageDrivenBean, MessageListener {
// This will be called for every message in the topic but only once across the cluster
public void onMessage(Message message) {}

// unimplemented methods
}



