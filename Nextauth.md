import NextAuth from "next-auth";
import GoogleProvider from "next-auth/providers/google";
import { PrismaAdapter } from "@next-auth/prisma-adapter";
import prisma from "../../../lib/prisma";

export default NextAuth({
  adapter: PrismaAdapter(prisma),
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID!,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
    }),
  ],
  pages: {
    signIn: '/auth/signin' // صفحتك الخاصة لتسجيل الدخول إن رغبت
  },
  theme: {
    brandColor: "#0ea5a4",
    logo: `${process.env.NEXT_PUBLIC_BASE_URL}/logo.png`,
    site: process.env.NEXT_PUBLIC_SITE_NAME || "Liumex"
  },
  callbacks: {
    async session({ session, user }) {
      session.user!.id = user.id;
      session.user!.role = (user as any).role;
      return session;
    }
  }
});
